---
layout: post
title: "OCUnit - Script to Kill Simulator After Tests Run"
date: 2012-10-10 09:55
categories: [OCUnit, Simulator]
author: Mark Struzinski
comments: true
---

I’m in the process of adding OCUnit tests to an existing iOS code base. Throughout this process, the biggest pain point I have encountered has been the simulator taking over the screen after an application test suite has run. 

<!-- more -->

This behavior completely breaks the flow of red/green/refactor, especially when you are on a roll. Here is a typical round trip for me when testing using the default behavior:

1. Make a code change
2. Hit **⌘U** to run the test suite
3. Simulator appears and takes over the screen
4. Tests complete in Xcode, which is now behind the simulator
5. **⌘ + Tab** back to Xcode to check the results, or **⌘ + Tab** to the simulator, then **⌘ + Q** to quit it

I solved this problem by running a script to close the simulator. To automate this, I hooked into Xcode’s behaviors functionality. Using behaviors, you can specify actions to run before and after testing runs. 

Here is a breakdown of how I am now forcing the simulator to close when tests complete:

- Open a text file and add the following shell script
``` bash Close Simulator Script
#!/bin/sh
osascript -e 'tell app "iPhone Simulator" to quit'
```
- Create a folder at the base level of your project named Scripts
- Save the text file into this folder and name it **quitsim.sh**
- Open a terminal session, cd to the new Scripts folder, and run the following command on the file to make it executable:

``` bash Make file executable

chmod +x quitsim.sh

```
- Now, open Xcode, and go to preferences(⌘,)
- Go to the Behaviors tab
- Under the Testing category, select Succeeds
- Scroll to the bottom, and check the Run selection
- Select **Choose Script ...** from the pull down menu
- Select the script you just saved into the Scripts folder
- Repeat this process for the Fails selection in the Testing category

You should now be able to run your test suite and have the simulator close upon completion or failure. This has removed a consistent annoyance for me and sped up my workflow just a little bit. 

Every small improvement helps.