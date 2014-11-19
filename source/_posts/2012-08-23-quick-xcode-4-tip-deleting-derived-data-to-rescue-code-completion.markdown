---
layout: post
title: "Quick Xcode 4 Tip: Deleting Derived Data to Rescue Code Completion"
date: 2011-09-30 07:50
categories: [Xcode, Tips]
author: Mark Struzinski
comments: true
---

##Quick Tip:##

I had an issue on a very large project in XCode. Starting 2 days ago, all of my syntax highlighting was completely broken. This is bad, but even worse, my code completion worked only partially. I was getting autocomplete for local variables only. When calling framework methods with a lot of parameters, I rely pretty heavily on code completion. I tried all of the usual fixes. 

<!-- more -->

This includes:
- Doing a __Project Clean (⌘⇧K)__
- Doing a __Clean Build Folder(⌥⌘⇧K)__
- Restarting XCode

None of these solutions fixed the issue. After some digging, I came across [this StackOverflow Post][stackoverflow] which pointed me in the right direction. Seems like sometimes the project index, which contains metadata used to display code completion suggestions (among other things), can become corrupted. The solution is to delete it. This will force XCode to regenerate it, which rebuilds the index.

Here are the steps to delete your Derived Data folder:

1. Go to XCode Settings __(⌘,)__
2. Click the Locations tab. You will see a screen like this:
{% img images/post_images/xcode-derived-data.png %}
3. Click the small arrow next to the location of the Derived Data folder. This will open the Finder to the location of the Derived Data for any XCode projects you currently have on your Mac.
4. Find the one that starts with the name of your current project and delete it.
5. You should immediately see XCode start to re-index your project.

For me, after this process completed, I got all of my functionality back. Your mileage may vary, and please make sure you back everything up before deleting anything!

[stackoverflow]:http://stackoverflow.com/questions/5365212/xcode-4-code-loses-syntax-coloring-when-importing-project-from-xcode-3/5368489#5368489