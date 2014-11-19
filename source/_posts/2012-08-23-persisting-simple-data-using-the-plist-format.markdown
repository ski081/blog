---
layout: post
title: "Persisting Simple Data Using the Plist Format: Part 1/1"
date: 2012-02-03 04:16
categories: [Persistence, Plist, Cocoa]
author: Mark Struzinski
comments: true
---

Recently, we had the need to save some simple transient data to disk. For data of any significant size, we would have looked at Core Data for our persistence. In this case, we decided to use the PList format to save data to disk and pull it back off. We went through several ideas to store this data.

Some of the ideas we considered for persistence included:

- Core Data
- JSON saved in a flat file format
- PList saved directly to disk from a Cocoa object such as an NSArray or NSDictionary

<!-- more -->

Because of our specific set of requirements and our hardware stack, we opted to go with the PList format. Our main driver for this decision was the amount of control we have over the response format from the server. Since we control our servers and their output, we are able to specify PList as a return type. We use the excellent [AFNetworking][afnetworking] library to perform our networking tasks.

The AFNetworking library has a request named [AFPropertyListRequestOperation][plistoperation].{% fn_ref 1 %} This operation will take an NSURLRequest and return an `id` that can be cast to a native Cocoa object, such as an NSDictionary or NSArray. Because we know the object’s return structure, we directly cast our object and move on to processing it. From there, it is just a matter of using the `writeToFile:Atomically` method to persist to disk.

Here is an example workflow:

```objective-c Save PList
NSString *urlString = @"http://api.responder?format=plist;"
NSURLRequest *urlRequest = [NSURLRequest requestWithURL:[NSURL urlWithString:urlString]];
    AFPropertyListRequestOperation *operation = 
    [AFPropertyListRequestOperation propertyListRequestOperationWithRequest:urlString success:^(NSURLRequest *request, NSHTTPURLResponse *response, id propertyList) {
            // Cache the response
        NSDictionary *responseDict = (NSDictionary *)propertyList;
        NSArray *pathArray = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES);
        NSString *cachesPath = [pathArray lastObject];
        NSString *filePath = [NSString pathWithComponents:[NSArray arrayWithObjects:cachesPath,@&quot;responseDict.plist&quot;,nil]];
        BOOL success = [responseDict writeToFile:filePath atomically:YES];
        if(!success){
            // Handle error
        }        
    } failure:^(NSURLRequest *request, NSHTTPURLResponse *response, NSError *error, id propertyList) {
        NSLog(@&quot;API Call failure: %@&quot;,[error localizedDescription]);
    }];
```

One caveat here is to be careful that your dictionary is properly formatted when being returned from the server, and when attempting to save to disk. Initially, we had some bugs when nulls were encountered as a response from the server for specific values. The server parsing logic would simply not generate a value for the key if the value was null. In PList format, this is unacceptable syntax. If you tried to save this to disk, the call would fail and return NO from the `writeToFile: method`.

For my next post, I’ll do a quick follow up on how to pull the data back off of disk and get it into a usable format in memory.

{% footnotes %}
  {% fn Please note that the headers returned from the server must specify application/x-plist data format. The AFPropertyListRequestOperation will check for this in the header and fail the request if it is not present. %}
{% endfootnotes%}

[afnetworking]:https://github.com/AFNetworking/AFNetworking
[plistoperation]:http://engineering.gowalla.com/AFNetworking/Classes/AFPropertyListRequestOperation.html