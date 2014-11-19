---
layout: post
title: Xcode File Template for Kiwi Tests
date: 2012-08-18 16:47
categories: [Xcode, Unit Testing, Kiwi]
author: Mark Struzinski
comments: true
---

I started using [Kiwi] to drive unit tests on my most recent project. Kiwi is an excellent [BDD (Behavior Driven Development)] framework that helps drive your code design by making test expectations very clear. It greatly increases the readability of your unit tests. 

<!-- more -->

The Kiwi library only requires implementation files to write each spec (test). Per the [documentation], you follow these steps to create a spec:

1. Create a .m file (no header required) 
2. Add an import for the Kiwi library `#import Kiwi.h`
3. Add the begin and end macros that expand at compile time to include the Kiwi code:
	1. `SPEC_BEGIN(SpecDescription)`
	2. `SPEC_END`
4. Inside the `SPEC_BEGIN()` and `SPEC_END` macros, write your tests

A typical Kiwi test follows this format:

``` objective-c Kiwi Test

describe(@“when “testing math”, ^{
    context(@"when adding 2 numbers", ^{
        it(@"should return the correct value", ^{
            int a = 1;
            int b = 3;
            int result = a + b;
            [[theValue(result) should] equal:theValue(4)];
        });
    });
});

```

Outside of the 4 lines of code that actually run the test condition, there is a lot of boilerplate code that has to be created to get up and running with a new test spec. I decided to take this opportunity to learn how to create a new Xcode file template. 

I found this excellent article on creating [custom Xcode file templates] by [Bob McCune]. You can read the link for greater detail, but basically you can create custom templates that will be picked up by Xcode’s New File dialog by creating this directory `~/Library/Developer/Xcode/Templates/File Templates/` and then placing each of your custom templates inside a folder in that directory. 

I created one that accepts a spec name and the first test decription. It creates a shell that includes the Kiwi import statement, the required macros, and the framework for 1 description, context, and test. The description{% fn_ref 1 %} is filled in based on your input in the new file creation dialog. I’ve created a [downloadable zip] of the required files. You should just be able to drop this into the location specified above and be up and running. 

As soon as possible, I’ll also share this on GitHub.

{% footnotes %}
  {% fn One issue I haven’t been able to work around: The description text replaces space characters with underscores. This is done by the compiler, and I have to research how to change this behavior (or update it’s outcome).  %}
{% endfootnotes%}

[Kiwi]: https://github.com/allending/Kiwi (allending/Kiwi)
[BDD (Behavior Driven Development)]: http://en.wikipedia.org/wiki/Behavior-driven_development (Behavior-driven development - Wikipedia, the free encyclopedia)
[documentation]: https://github.com/allending/Kiwi (allending/Kiwi)
[custom Xcode file templates]: http://www.bobmccune.com/2012/03/04/creating-custom-xcode-4-file-templates/
[Bob McCune]: http://www.bobmccune.com
[downloadable zip]: http://cl.ly/1w3i1H2p1h0z