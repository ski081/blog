---
layout: post
title: "Quick Wipe of iOS Simulator Using Alfred"
date: 2012-05-02 07:37
categories: [Simulator, Tips, Alfred]
author: Mark Struzinski
comments: true
---

When I am rapidly prototyping new features, I frequently need to blow out the entire app structure from the simulator and start fresh.

To do this manually, I would follow the following steps.

From Simulator (when simulator is open)*:
1. Go to Menu Bar
2. Click iOS Simulator
3. Click Reset Content and Settings

<!-- more -->

###Benefits of this approach:###

- Easy to do/remember
- Blows out all settings and apps

###Drawbacks to this approach###

- Destroys ALL content, including any general settings     
- I frequently need to test something more complex, like uploading images. I will save images to the simulator by going to Safari on the simulator and saving images form the web for use in the simulator&rsquo;s photo library. Using the Reset Content option destroys all of this data and it has to be recreated.
- Simulator must already be running to reset settings

##From Finder##

- Go to path: __~/[user]/Library/Application\ Support/iPhone\ Simulator/[SDK Version]/Applications__
- Delete all folders in the Applications folder

###Benefits of this approach###

- Deletes apps from simulator without wiping settings

###Drawbacks to this approach###

- Very manual, even when you have a sidebar shortcut set up in the Finder

##My solution: Alfred Simulator Wipe##

I love the application [Alfred][alfred]. Alfred is an extremely useful app that started its life as an application launcher, and has evolved into a swiss army knife that is ultimately customizable and expandable. When I get on any Mac and hit __⌘-space__ and see Spotlight instead of Alfred, I know my productivity will be less than normal. One of the best features of Alfred is the ability to create custom shell/Applescript extensions and run them from the app with a keyword (The devs just added the ability to pipe input into these scripts with v1.2, but I haven&rsquo;t had a chance to play with it yet). This is the technique I used to automate the majority of this simulator wipe process (You have to have purchased the PowerPack to take advantage of extensions). Here is how to create an Alfred extension to wipe your Simulator applications:

1. [Download Alfred][download-alfred]
2. Open Alfred Preferences (with Alfred open, hit __⌘,__)
3. Click the '+' button at the bottom left of the window:
{% img /images/post_images/wipe-sim-apps.png %}
4. Select AppleScript
5. Enter an Extension Name. Other details are optional.
6. To make it look nice in Alfred, you can drag in an icon that will be displayed in the app when the keyword is invoked
7. Enter a title and description
8. Pick a keyword that Alfred will use to launch the script
9. Check the __Background__ box (This allows the script to run and not block Alfred until it completes)
10. Use the following code in the AppleScript field:
```applescript Alfred AppleScript
tell application &ldquo;Finder&rdquo;
delete (every item of folder &ldquo;[Drive]:Users:[user]:Library:Application Support:iPhone Simulator:5.1:Applications&rdquo;)
end tell
```
11. The iOS simulator uses a separate directory for each installed SDK. For each iOS SDK on your system (I have 3), add a new line to delete that Simulator&rsquo;s installed apps. You would simply copy the delete line above and change [5.1] to a different folder name.
12. Click the Save button when you're done
13. Invoke Alfred, then type in the keyword you set up to run the extension
14. You should wind up with something like you see below
{% img /images/post_images/alfred-wipe-in-action.png %}

This will easily wipe out your simulator apps without destroying any data in the simulator itself. It&rsquo;s also a lot easier than going through the Finder every time. I&rsquo;m linking to an export of the extension below. Please let me know in the comments if you are doing anything similar or you can think of any improvements!

[Alfred Simulator Wipe Extension][download-extension]

[alfred]: http://www.alfredapp.com/
[download-alfred]: http://www.alfredapp.com/#download-alfred
[download-extension]: http://cl.ly/0w300M1W2B253P412n08