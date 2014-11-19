---
layout: post
title: "Setting Up iOS Logic Tests [Part 2]"
date: 2013-02-13 15:02
comments: true
categories: [Unit Testing Series]
author: Mark Struzinski
---

This is part 2 in a [multi-part series](/blog/2013/02/01/unit-testing-series/) on iOS unit testing and integration testing. In the [last post](/blog/2013/02/08/setting-up-ios-logic-tests/), we discussed setting up the project and adding some dependencies with CocoaPods. 

Today, I’m going to go through setting up some initial code to use the 3rd party libraries to make sure that the libraries are working. Then we’ll set up logic tests and see what breaks with CocoaPods (spoiler: compiler errors ahead!).

<!-- more -->

## SVProgressHUD

First, let’s hook up [SVProgressHUD](https://github.com/samvermette/SVProgressHUD)

1. Open `ViewController.m` and import SVProgressHUD: `#import "SVProgressHUD.h”`
2. In `viewDidLoad`, create an SVProgressHUD indicator, then dismiss it after 2 seconds:    
``` objective-c ViewController.m
		-(void)viewDidLoad{
			[super viewDidLoad];
			[SVProgressHUD showWithStatus:@"Running..."];
	    
	    	double delayInSeconds = 2.0;
	    	dispatch_time_t popTime = dispatch_time(DISPATCH_TIME_NOW, (int64_t)	(delayInSeconds * NSEC_PER_SEC));
	    	dispatch_after(popTime, dispatch_get_main_queue(), ^(void){
	        	[SVProgressHUD dismiss];
	    	});
	    }
```

You should be able to run this code now and see a progress indicator with a spinner and the text “Running....”. It should disappear after 2 seconds. 

{% img /images/post_images/logictests-svprogresshud-spinner.png %}

Great! This proves one of our CocoaPods libraries is communicating with the app’s main project and is working properly. 

Next we’ll move onto Core Data with Magical Record.

[github]: https://github.com/samvermette/SVProgressHUD