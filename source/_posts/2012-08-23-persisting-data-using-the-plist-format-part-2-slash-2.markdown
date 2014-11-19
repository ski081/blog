---
layout: post
title: "Persisting Data Using the Plist Format: Part 2/2"
date: 2012-02-23 12:16
categories: [Persistence, Plist, Cocoa]
author: Mark Struzinski
comments: true
---

This is a follow up to my [previous post]:[part1] on persisting PList data to disk as a form of transient storage for data. This second part will just show an easy way to pull that data from disk and get it into memory in an easily usable format. 

All of our Plists are stored as dictionaries. The keys for the dictionaries are stored as constants in a code file (`Constants.h/.m`). This allows for compile time checking and keeps us from having to litter our code with magic strings when trying to access the values.

<!-- more -->

Here is a sample workflow to get this method up and running:

##The PList Data##

Assuming we have a plist that contains the following data:

```xml PList Data
<xml version="1.0&quot; encoding="UTF-8">
<DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>firstName</key>
    <string>Mark</string>
</dict>
</plist>
```

##Create a Constants File, and Always Use It##

Create a Constants file, and add all keys to it as `const NSString`. Be strict and only reference these keys when attempting to access data from your NSDictionaries. Don’t shortcut and put the strings directly into your code. Here is an example of how I have this set up:

####Constants.h####

```objective-c Constants Header
#import <Foundation/Foundation.h>;

@interface Constants : NSObject

extern NSString * const DICT_KEY_FIRST_NAME;

@end
```

####Constants.m####

```objective-c Constants Implementation
#import "Constants.h";

@implementation Constants

NSString * const DICT_KEY_FIRST_NAME   = @"firstName";

@end
```

##Add your Constants file to your pre-compiled headers file so it is available everywhere##

Adding files to your pre-compiled headers makes them available everywhere in your project without having to import each specific class header in the file where you’re trying to use it. Use this convenience judiciously, and only for classes that you truly think you’ll need everywhere. I always include my Constants file in the pre-compiled header to ensure I can access it without having to import it everywhere.

You can find your pre-compiled header file in the __Supporting Files__ group in your project navigator in XCode (if you haven’t moved it). It is typically named `{projectname}-Prefix.pch`, where {projectname} is the name of your project. At the bottom of this file, you can add #import statements for the header fields you are interested in. Your file should look something like this:

```objective-c Prefix Header

#import <Availability.h>

#ifndef __IPHONE_3_0
#warning &quot;This project uses features only available in iOS SDK 3.0 and later.&quot;
#endif

#ifdef __OBJC__
    #import <UIKit/UIKit.h>
    #import <Foundation/Foundation.h>
    #import <CoreData/CoreData.h>
    #import "Constants.h"
#endif
```

The import statement you’re interested in the last one. Make sure you add it inside the `#ifdef__OBJC__` block.

This adds an #import statement for each file at compile time which includes these files throughout your entire project. It will also update XCode’s autocomplete indexing so that you are able to use autocomplete for the code on the imported files.

##Pull your file from disk, and use the constants defined##

Here is an example of how we could wrap this all up and pull some data from a plist file already on disk:

```objective-c Retrieve Data
NSString *filepath = [self convenienceMethodToGetFilePathForPlist];
NSDictionary *resultsDict = [NSDictionary dictionaryWithContentsOfFile:filepath];
NSString *firstName = [resultsDict objectForKey:DICT_KEY_FIRST_NAME];
NSLog(@"First Name: %@",firstName);
```

Note above that you will need a way to get the plist file path before being able to access it. Also note that I prefix the string constant with `DICT_KEY`. I do this so that as my constants file grows, I can easily locate the value I’m looking for by typing the prefix I’m looking for.

Well, that wraps up this segment. As a caveat here, I’d like to mention that this data storage methodology, in my opinion, would not scale to larger data sets. You should definitely consider using [Core Data]:[coredata] for the majority of your data storage scenarios. We decided to use the PList format in this case because the data was transient, and was going to be wiped out on each subsequent change to it.

[part1]: http://markstruzinski.com/blog/2012/02/03/persisting-simple-data-using-the-plist-format/
[coredata]:https://developer.apple.com/library/mac/#documentation/cocoa/conceptual/coredata/cdprogrammingguide.html