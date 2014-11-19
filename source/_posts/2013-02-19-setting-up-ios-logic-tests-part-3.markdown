---
layout: post
title: "Setting Up iOS Logic Tests [Part 3]"
date: 2013-02-19 13:24
comments: true
categories: [Unit Testing Series]
author: Mark Struzinski
---

This is part 3 in a [multi-part series](/blog/2013/02/01/unit-testing-series/) on iOS unit testing and integration testing. In the [last post](/blog/2013/02/13/setting-up-ios-logic-tests-part-2/), we discussed setting up [SVProgressHUD](https://github.com/samvermette/SVProgressHUD). 

## Magical Record
[Magical Record][magicalrecord] is an excellent library that compliments the Core Data framework. I’m going to assume some knowledge of Core Data here. If you need a reference, the [Core Data][amazon] book by Marcus Zarra is excellent, and just hit its 2nd edition. We are going to build out a very simple data model, with just one entity. This will allow us to set up the core data stack and verify that Magical Record is working.

<!-- more -->

Let’s get core data set up:

- **Add the Core Data framework in the Link Binary with Libraries step**
	1. Click the project node in the project navigator sidebar on the left
	2. Select the app target
	3. Select the Build Phases tab
	4. Open the Link Binary with Libraries list
	5. Click the **‘+’** button
	6. Search for **‘Core Data’**
	7. Select `CoreData.framework`
	8. Click the **Add** button
	
{% img /images/post_images/logictests-coredata-framework.png %}
	
- **Add an import for Core Data to the precompiled header file to make Core Data available for the entire project**
	1. Open the precompiled header file - This file is usually placed in the **Supporting Files** group, and is named [project-name]-Prefix.pch
	2. Inside the `#ifdef __OBJC__` block, add `#import <CoreData/CoreData.h>`. This will make core data available throughout the project without having to import the framework everywhere it needs to be used.
	
- **Add a data model to your app**
	1. Right click on the group in the project navigator that represents your app. For me, this is **LogicTests**
	2. Select **New File**
	3. Select Core Data => Data Model
	4. Select Create
	5. Your new data model should open in the graphical editor. By default, it will be named **Model**

- **Create a Person entity**
	1. On the bottom toolbar, click the **Add Entity** button
	2. A new entity will be created and the name will be editable immediately in the left sidebar. Type **Person** and hit Return
	3. We now have a new entity. We need to create attributes for it. 
	4. Under the **Attributes** section, click the ‘+’ symbol. Name the new attribute `firstName` and make it a `String` type
	5. Repeat this process, and create a `lastName` property, also of type `String`
	6. When you finish, your entity should look like this:
	
	{% img /images/post_images/logictests-person-entity.png %}

- **Create an NSManagedObject subclass**{% fn_ref 1 %}
	1. With your newly created **Person** entity selected, go to the menu bar, and select Editor => Create NSManagedObject Subclass
	2. Leave everything as-is, and click Create
	3. You should now have a `Person.h` and `Person.m` file in your project navigator. Inspecting these files will just show some standard property declarations in the header, and `@dynamic` declarations in the implementation.  Without diving too deep here, the `@dynamic` declarations simply tell the compiler that the implementation is there for `NSManagedObject` subclasses and prevent compiler warnings.
	4. Let’s take a sanity break here and hit **⌘B** to make sure the app builds.

- **Use Magical Record to set up the Core Data stack**

Magical Record is an excellent helper library that makes interacting with Core Data much easier. It adds syntax that allows you to perform common operations in 1 line of code instead of 3 or 4. Let’s add Magical Record into the mix and use it to bootstrap our Core Data stack.

1. Add an import to your .pch file so that Magical Record is available project wide. My .pch file now looks like this:

``` objective-c LogicTests-Prefix.pch
		#import <Availability.h>
	
		#define MR_SHORTHAND
		#ifdef __OBJC__
			#import <UIKit/UIKit.h>
			#import <Foundation/Foundation.h>
    		#import <CoreData/CoreData.h>
    		#import "CoreData+MagicalRecord.h"
		#endif
```
 
- The `#define` statement allows you to use Magical Record calls without a prefix. Without this define, you would have to prefix all of your Magical Record calls with `MR_`
- The `#import "CoreData+MagicalRecord.h"` statement adds categories to several Core Data classes, which allow you to use the Magical Record extensions

2. In your app delegate, add the following statement to set up the core data stack:
	` [MagicalRecord setupCoreDataStack];`
3. Build to make sure everything is wired up correctly and you get no compiler errors.

Ok, now we’re all set up. Next, we need to create the unit test bundle and begin to meet the challenges of creating unit tests against our code.

I’ve put my progress so far [up on GitHub for reference][github]. Please feel free to check it out!

{% footnotes %}
	{% fn %} I always create `NSManagedObject` subclasses. I prefer to use [Mogenerator](https://github.com/rentzsch/mogenerator). Mogenerator is a much more elegant and destruction-proof way of doing this, but for the sake of brevity, we’ll just use the built-in class generation from the Core Data modeler.
{% endfootnotes %}

[amazon]: http://www.amazon.com/gp/product/1937785084/ref=as_li_qf_sp_asin_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1937785084&linkCode=as2&tag=markstruz0a-20
[github]: https://github.com/ski081/LogicTests
[github 2]: https://github.com/rentzsch/mogenerator
[magicalrecord]: http://magicalrecord.com/