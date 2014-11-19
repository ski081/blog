---
layout: post
title: "Unit Testing Series"
date: 2013-02-01 16:31
comments: true
categories: [Unit Testing, Frank, Continuous Integration, OCUnit, Automation, Unit Testing Series]
author: Mark Struzinski
---

I have wanted to get better at unit testing and the tooling around it for some time. I usually start out determined to get a good amount of the code covered by unit tests, and to possibly get some UI tests built around user interactions. Unfortunately, deadlines intervene, and the tests get abandoned. With my most recent project, I decided to put all of these practices in place. 

<!-- more -->

This is going to require learning as I go, and I intend to document my progress here for my own learning and hopefully to help anyone else who is researching the same thing.

I intend to use the following technologies:

- Unit testing using standard the OCUnit/SenTest frameworks included with Xcode
	- I’ve flirted with other projects in the past, including [Kiwi][github] . I really like the syntax and organization that Kiwi provides, but I want to stick with the out of the box stuff for now
- Integration/UI testing with [Frank][testingwithfrank]. I’ve got a fair amount of experience with Frank. I’ve also played with [KIF][github 2], and much prefer the separation and tooling that Frank provides
- Continuous Integration using [Jenkins][jenkins-ci]
- Build versioning using a build script for automated version increments for release builds

I’m going to try to document my thought process and all the steps I take along the way. Hopefully, I can get some constructive feedback on any improvements I can make.

[github]: https://github.com/allending/Kiwi
[github 2]: https://github.com/square/KIF
[jenkins-ci]: http://jenkins-ci.org/
[testingwithfrank]: http://testingwithfrank.com/