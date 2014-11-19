---
layout: post
title: "CodePilot"
date: 2012-12-12 16:59
categories: [CodePilot, Utilities, Xcode]
author: Mark Struzinski
---

 I’ve been using a handy little utility named [CodePilot](http://codepilot.cc) since Xcode 3.x. CodePilot runs as a plugin and does a few things simply. It will open any file that has been indexed in your project, and also allows for searching symbols within a file. 
 
<!-- more -->

I know the native Xcode Open Quickly (⌘ + ⬆ + O) dialog has also been able to do this for a long time, but it has always seemed to me to be a little bit slower and less intuitive than CodePilot. 

## Usage ##
Using CodePilot is simple. From anywhere within Xcode, type (⌘ + ⬆ + X) to open a search box. The search interface looks like this:

{% img /images/post_images/codepilot-ui.png %}

Compare that to the Open Quickly functionality already bundled with Xcode:

{% img /images/post_images/xcode_open_quickly.png %}

Typing a phrase that matches any part of the file name will bring up a list of matches. Subtext on the matches shows you what group the files resides in. You use the arrow keys to move up or down the list and press enter to open the file in the frontmost Xcode window. By default, when the interface launches, it will display a list of your most recently accessed files descending by time opened.

If you want to perform a quicker search based on the camel-cased uppercase letters in the file name (ex: for ViewController.m, you can type **‘VC’**). Note that the CodePilot UI also underlines the letters it is matching on, which is a nice touch:

{% img /images/post_images/codepilot-fuzzysearch.png %}

You can search for symbols within a file as well. This includes method names, properties, ivars, and other identifiable symbols. For example, if you search for a file named ViewController.m and find it, you can arrow down to it, hit the space bar, then start typing the name of a symbol, such as **‘aw’** for `awakeFromNib`:

{% img /images/post_images/codepilot-symbolsearch.png %}

## Settings ##
CodePilot offers a small list of settings. You can configure:

- Remember last search (prefills the box with your last search result)
- Auto search for selected text from Xcode

CodePilot offers productivity gains by keeping your hands off of the mouse. It is one of the few utilities that I really miss when working on someone else’s system. Recently, Codepilot went free, which makes me worry for its future. Since it is an Xcode plugin, every time Xcode updates to a new version, there is a good possibility it will break. This has happened in the past, most notably when Xcode moved from v3 to v4. 

Hopefully, there will be no breaking changes in future Xcode builds. Until then, it’s a great little utility that help speed me up just a little bit throughout my day.



