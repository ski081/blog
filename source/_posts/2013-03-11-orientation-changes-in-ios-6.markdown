---
layout: post
title: "Orientation Changes in iOS 6"
date: 2013-03-11 11:05
comments: true
categories: [orientation]
author: Mark Struzinski
---


In a current project, I had the need to have one specific view controller present its view in landscape orientation only. Pre-iOS 6, I would have overridden the `shouldAutoRotateToInterfaceOrientation` method and returned `UIInterfaceOrientationLandscape`. In iOS 6, this method is deprecated. I began researching how orientation issues should be handled going forward, and here is the way I made my specific scenario work.

<!-- more -->

In iOS 6, the system queries the **topmost** visible view controller to see if it should rotate. The viewcontroller can override 2 methods to answer this query correctly:

* `supportedInterfaceOrientations:`
* `shouldAutoRotate`

It would seem simple enough to override these methods on each view controller that had special requirements for presentation to have them determine their orientation. The problem arose for me when I had a navigation controller thrown into the mix. Since technically the topmost view controller in a navigation stack is the navigation controller itself, iOS was ignoring the method overrides I had in place for the individual view controllers. It was sending the message directly to my UINavigationController, which did not provide the settings I needed.

I solved this issue by subclassing UINavigationController and overriding the previously mentioned methods, then passing the results from the navigation controller down into the top view controller via the `topViewController` property. Here are the steps to accomplish this:

1. Create a subclass of `UINavigationController`
2. Override the following methods:
	1. `-(UIInterfaceOrientation)preferredInterfaceOrientationForPresentation`
	2. `-(NSUInteger)supportedInterfaceOrientations`
	3. `-(BOOL)shouldAutorotate`
3. Return the result of each of these operations by passing in the value from the result of calling that method on the topViewControllerof the UINavigationController
4. Implement any or all of those methods specific to your needs on your UIViewController subclasses
5. Make sure you are using your `UINavigationController` subclass in place of the standard `UINavigationController`
	1. My project uses storyboards, so I simply specified my UINavigationController subclass in the Identity Inspector

Here is my implementation of the navigation controller subclass in its entirety:

``` objective-c UINavigationController Subclass

	@implementation FMNavigationController

	-(BOOL)shouldAutorotate{
    	return [self.topViewController shouldAutorotate];
	}

	-(NSUInteger)supportedInterfaceOrientations{
    	return [self.topViewController supportedInterfaceOrientations];
	}

	-(UIInterfaceOrientation)preferredInterfaceOrientationForPresentation{
    	return [self.topViewController preferredInterfaceOrientationForPresentation];
	}

	@end


```

Then, in my specific view controller that needs to be launched in landscape, I overrode this method and returned the appropriate value:

``` objective-c View Controller Method Overrides

	#pragma mark - Rotation
	-(NSUInteger)supportedInterfaceOrientations{
    	return UIInterfaceOrientationMaskLandscape;
	}

```

That’s really all there was to it. The research to get this right took much longer than the implementation. 

Please contact me with any questions, and good luck.