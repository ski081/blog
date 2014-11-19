---
layout: post
title: "Unit Testing Patterns for Asynchronous Networking Communications"
date: 2013-10-10 06:59
comments: true
categories: [Unit Testing]
author: Mark Struzinski
---

A common issue I always run into is how to test asynchronous methods, especially networking calls. I used test the result of the calls, such as the parsing of return data, because testing the entire method proved impossible. I recently read an [article](http://www.objc.io/issue-2/async-testing.html) from the excellent [objc.io](http://www.objc.io/) publication on asynchornous testing. By combining the patterns used in this article with some refactoring, I finally have my networking code under unit tests. Here's what I did.

<!-- more -->

### Architecture
First, I will detail how my app was architected **prior** to setting up unit tests. I use [AFNetworking](https://github.com/AFNetworking/AFNetworking) to handle all API calls, and make use of the [AFHTTPClient](http://engineering.gowalla.com/AFNetworking/Classes/AFHTTPClient.html) to centralize all calls to a specific base URL. The HTTPClient would be a singleton, and each service call would be passed a typed return block to execute on completion or failure. Here is an example for a service call to get a list of items:

In my header:

```objective-c APIClient.h
@interface APIClient : AFHTTPClient

typedef void(^ListItemsRequestReturnBlock)(BOOL success,NSArray *itemsArray,NSError *error);

-(void)sendItemsRequestForGroupId:(NSNumber *)groupId completionBlock:(ListItemsRequestReturnBlock)itemsReturnBlock;

@end
```

And the implementation:

```objective-c APIClient.m
-(void)sendItemsRequestForGroupId:(NSNumber *)groupId completionBlock:(ListItemsRequestReturnBlock)itemsReturnBlock{
    Group *group = [Group findFirstByAttribute:GroupAttributes.groupID
                                        withValue:groupId];
    
    NSDictionary *paramsDict = @{
                                 API_PARAMETER_KEY_ACTION           : API_PARAMETER_ACTION_VALUE_ITEMS_REQUEST,
                                 API_PARAMETER_KEY_GROUP           : groupId
                                 };
    
    [[SalesCoachAPIClient sharedClient] getPath:nil
                                     parameters:paramsDict
                                        success:^(AFHTTPRequestOperation *operation, id responseObject) {
    									NSArray *lessonClassesArray = nil;
    									if ([self checkAPIResponseDictForSuccess:responseObject]) {
        									itemsArray = [Item itemsArrayFromAPIDict:responseObject];
        
        									if (itemsArray) {
            									[group setItems:[NSSet setWithArray:itemsArray]];
            									[[NSManagedObjectContext defaultContext] saveToPersistentStoreAndWait];
        									}
    									}
                                            if (itemsArray) {
                                                itemsReturnBlock(YES,itemsArray,nil);
                                            }else{
                                                itemsReturnBlock(NO,nil,nil);
                                            }
                                        } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
                                            itemsReturnBlock(NO,nil,error);
                                        }];
    
}
```

This same pattern is used each time I want to make a call into my API, and a new call is set up for each endpoint. The pattern works well, and has proven itself useful. However, it always felt like the APIClient class was doing a little too much work, and had too much knowledge of the internals of parsing the JSON data returned. This proved to be true once I attempted to apply tests to these methods. 

### Building the Tests

The first lesson learned: always test and code at the same time (preferably in that order)! This is about the tenth time I have told myself I was too time constrained to write tests while I was coding. In hindsight, it usually means I'll be doing some heavy refactoring after I start writing tests. The tests always show that the code I wrote quickly is too interdependent, and needs to be broken down more into logical components. 

In order to separate concerns and to facilitate mocking data parsing responses, I began to move data parsing code and logic into a separate helper class. This class would be responsible for the following:

1. Parse the JSON response
2. Report success ior failure
3. Create and populate data model objects out of the response
4. Save the objects to core data
5. Return core data object(s) from the call after parsing and saving is complete

So with that in mind, the above code changes implementation to look like this:

```objective-c APIClient.m
-(void)sendItemsRequestForGroupId:(NSNumber *)groupId dataManager:(DataProcessingManager *)dataManager completionBlock:(ListItemsRequestReturnBlock)itemsReturnBlock{
    Group *group = [Group findFirstByAttribute:GroupAttributes.groupID
                                        withValue:groupId];
    
    NSDictionary *paramsDict = @{
                                 API_PARAMETER_KEY_ACTION           : API_PARAMETER_ACTION_VALUE_ITEMS_REQUEST,
                                 API_PARAMETER_KEY_GROUP           : groupId
                                 };
    
    [[APIClient sharedClient] getPath:nil
                                     parameters:paramsDict
                                        success:^(AFHTTPRequestOperation *operation, id responseObject) {
                                            NSArray *itemsArray = [dataManager itemsArrayForResponseObject:responseObject];
                                            if (itemsArray) {
                                                itemsReturnBlock(YES,lessonClassesArray,nil);
                                            }else{
                                                itemsReturnBlock(NO,nil,nil);
                                            }
                                        } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
                                            itemsReturnBlock(NO,nil,error);
                                        }];
    
}
```

This allows me to mock the DataProcessingManager object and tell it what to return. Putting this all together, I now have a way of testing asynchronous API calls with test data and validating that my networking calls are doing the right thing in response to different scenarios. After this change, one of my tests against this networking call would look like this:


```objective-c APIClientTests.m

#import <SenTestingKitAsync/SenTestingKitAsync.h>
#import <OCMock/OCMock.h>
#import <OHHTTPStubs/OHHTTPStubs.h>

-(void)testGetItemsRequestSuccessAsync{
    [self setupStubsRequestForResponseName:@"items_request"];
    id manager = [OCMockObject mockForClass:[DataProcessingManager class]];
    NSArray *items = @[@1,@2];
    [[[manager stub] andReturn:items] itemsArrayForResponseObject:OCMOCK_ANY];
    
    [[SalesCoachAPIClient sharedClient] sendItemsRequestForGroupId:@1
                                                              dataManager:@"ski081"
                                                       completionBlock:^(BOOL success, NSArray *itemsArray, NSError *error) {
                                                           STAssertTrue(success, nil);
                                                           STAssertNil(error, nil);
                                                           STAssertNotNil(itemsArray, nil);
                                                           STAssertTrue(itemsArray.count == 2, nil);
                                                           STSuccess();
                                                       }];    
}

```

I am making use of the following helper methods in my test class:

```objective-c APIClientTests.m

#pragma mark - Helper
-(OHHTTPStubsResponse *)stubResponseForResponseNamed:(NSString *)responseName responseTime:(CGFloat)responseTime{
    return [OHHTTPStubsResponse responseNamed:responseName
                                   fromBundle:self.bundle
                                 responseTime:responseTime];
}

-(void)setupStubsRequestForResponseName:(NSString *)responseName{
    OHHTTPStubsResponse *response = [self stubResponseForResponseNamed:responseName
                                                          responseTime:0.1f];
    [OHHTTPStubs stubRequestsPassingTest:^BOOL(NSURLRequest *request) {
        return YES;
    } withStubResponse:^OHHTTPStubsResponse *(NSURLRequest *request) {
        return response;
    }];
}

```

### Final note
To enable testing asynchronous calls, I relied heavilt on the excellent [2nd issue](http://www.objc.io/issue-2/editorial.html) of [objc.io ](http://www.objc.io). From that article, I was introduced to the [OHHTTPStubs](https://github.com/AliSoftware/OHHTTPStubs)  and [SenTestingKitAsync](https://github.com/nxtbgthng/SenTestingKitAsync) library. 

Some other libraries I am making use of here:
- [MagicalRecord](https://github.com/magicalpanda/MagicalRecord)
- [OCMock](https://github.com/erikdoe/ocmock)

This is a work in progress, but has been the best thing I've been able to come up with so far. It definitely relieves some pain for me. 

Best of luck testing, and drop me a comment here with any additional tips/suggestions!

