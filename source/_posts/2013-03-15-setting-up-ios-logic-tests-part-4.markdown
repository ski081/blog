---
layout: post
title: "Setting up iOS Logic Tests [part 4]"
date: 2013-04-08 16:05
comments: true
categories: [Unit Testing Series]
author: Mark Struzinski
published: true
---

This is part 4 in a [multi-part series](/blog/2013/02/01/unit-testing-series/) on iOS unit testing and integration testing. In the [last post](/blog/2013/02/19/setting-up-ios-logic-tests-part-3/), we discussed setting up Core Data and the Magical Record library. This week, we’re going to set up our logic testing bundle. Let’s get started.

<!-- more -->

**Quick note: I recreated the sample project that goes along with this series. I made some unfortunate naming mistakes early on that made it unclear which bundles we were refernecing and working with. Going forward, the actual app bundle is named “SampleApp”, and other testing bundles will be named appropriately. I’m taking down the old GitHub repo and adding one here named SampleApp

I went through and cleaned up the project a little bit. I moved some files into subfolders to get it a little more organized, and chanfed the name of the app target to `SamleApp` to make things a little clearer. The project is up on [GitHub](https://github.com/ski081/LogicTests) and you can simply do a pull to get the changes.

## Adding a Logic Test Bundle

First, we need to add the logic test bundle. Open the project navigator, and select the project node at the very top. In the center window, click the **Add Target** button at the bottom of the screen. Under the iOS section in the left menu, select **Other**. In the center pane, select **Cocoa Unit Testing Bundle**:

{% img /images/post_images/logictests-select-bundle.png %}

Name it **“Logic Tests”**, leave everything else as-is, and click Finish. You should now be able to run the default unit tests included with the bundle and get a failing result. This will verify that your tests are set up. To verify:

1. Switch your target in the top drop down from **“SampleApp”** to **“LogicTests”**
2. Go to the menu bat, select **Product=>Test**
3. You should see a new issue in the the Issue Navigator that reads “*Unit tests are not implemented yet in LogicTests*”
4. If not, review the setup steps and try to figure out of you missed something

Next, we should make it easier to run these tests. As it is, every time you want to run your logic tests, you would have to switch targets from your SampleApp target to the LogicTests target, then run the tests. This is harldy easy or efficient. We can make this simpler by hooking up the Test action of our main app scheme to the LogicTests bundle. This will enable is to run the logic tests directly from the main app bundle. Here’s how to do that:

1. Select the SampleApp target
2. Click the drop down, and select **“Edit Scheme”**
3. Select the Test action in the left sidebar
4. Click the ‘**+**’ button to add an action
5. You should see your LogicTests bundle under the SampleApp project. Select it, and click **Add**
	{% img /images/post_images/logictests-add-bundle-action.png %}
6. Click Ok

Finally, instead of using the menu item to run unit tests, a much easier way to trigger them is via a keyboard shortcut. In Xcode, the default keyboard shortcut to run unit tests against the current target is to simply hit **⌘ + U**. Try this while you have SampleApp selected. It should run the LogicTests bundle and take you to the same error in the issue navigator as before. 

That’s it for this installment. Next up, we’ll start making use of our logic tests and look at the differences between logic tests and application tests in Xcode.

As always, please contact me with any questions.