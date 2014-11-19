---
layout: post
title: "Setting up iOS Logic Tests [Part 1]"
date: 2013-02-08 11:47
comments: true
categories: [Unit-Testing-Series]
author: Mark Struzinski
---

I'm continuing on my task to get a full project using iOS unit tests and integration tests. My first step is to set up logic tests in Xcode. I recently watched an excellent [unit testing course][lynda] on [Lynda][lynda 2]. In that course, Ron Lisle goes over the advantages of using logic tests. The most compelling factor in using logic tests over application tests is speed.

<!-- more -->

Application tests actually bootstrap the entire app in order to run. This takes a considerable amount of time and can be a bottleneck. You want your unit tests to run fast. Logic tests will build the main application bundle, but then run in isolation and tests methods on any class you are interested in. I decided to write as many logic tests as possible, and only switch to application tests when logic tests could not get the job done. I am anticipating that I will need to use application tests once I start testing view controllers, but I won’t know until I get there.

## Setting up the project ##
I am starting from a blank slate, but I have one caveat: I want to use [CocoaPods][cocoapods] for all of my external library maintenance. If you haven’t heard of CocoaPods, you should read up on it. CocoaPods is a way to manage your 3rd party dependencies more easily. It supports updating libraries in place and removes a lot of pain with integrating 3rd party code. As you will see, however, using CocoaPods with logic tests makes things a little more complex during setup. 

To begin, start a new project without any unit tests included. 

{% img /images/post_images/logictests-project-setup.png %}

Next, make sure you have the cocoa pods gem set up . There are directions [CocoaPods Setup][cocoapods] if you’ve never done this before. After that, let’s bring in 2 3rd party libraries by creating a podspec file and telling CocoaPods to set everything up for us.

## Set up a Podspec file ##
1. Close Xcode if it’s still open
2. Create a file in the base folder of your project named `Podfile` with no extension
2. Add a line telling CocoaPods the platform and version of iOS you are using.
	1. `platform :ios, '6.0'`
3. Add lines for 2 pods (these are really dependencies on 3rd party code)
	1. `pod ‘SVProgressHUD’`
	2. `pod ‘MagicalRecord’`
4. Save your file and close it
5. Open terminal, navigate to the base folder of your project, and enter the following line
	1. `pod install`
6. You should see some status lines go by, then a line instructing you to use a workspace now instead of the project file. This is because CocoaPods creates a second project named `Pods` and adds it and your original project to a workspace named after your project. In my case, it created a `LogicTests.xcworkspace file`. If I open that file now, I can see that I have a `Pods` project and a `LogicTests` project inside of the workspace.

{% img /images/post_images/logictests-cocoapods-project.png %}

We're going to break here for now. Next, we'll start making use of the 3rd party libraries we included via CocoaPods. After that, we'll finally set up the logic tests and get them running.

[cocoapods]: http://cocoapods.org/ "CocoaPods: The Objective-C Library Manager"
[lynda]: http://www.lynda.com/iOS-tutorials/Unit-Testing-iOS-Applications-Xcode/91949-2.html?srchtrk=index%3a0%0alinktypeid%3a2%0aq%3aios+unit%0apage%3a1%0as%3arelevance%0asa%3atrue%0aproducttypeid%3a2
[lynda 2]: http://www.lynda.com