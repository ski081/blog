---
layout: post
title: "Keyboard Maestro macro to open in browser from ReadKit"
date: 2013-01-16 12:48
comments: true
categories: [ReadKit, Keyboard Maestro]
author: Mark Struzinski
---

I recently switched to [ReadKit](http://www2.ed.gov/pubs/CompactforReading/index.html) for reading [Instapaper](https://www.instapaper.com/) articles on my mac. I switched over from Read Later (no longer available). Read Later was my app of choice for a long time, but it is no longer in active development. The team was hired by [Pocket](http://www.pocketmac.com/), and they are working solely on the mac app now. Read Later was good, but some much needed updates were never implemented. The display for articles was frequently skewed, and the app had started to crash a lot.

<!-- more -->

ReadKit’s display of articles is much smoother, and the overall look and feel is fresh and crisp. One thing I do miss from Read Later is the keyboard shortcuts. I frequently need to open articles in a browser quickly. Whenever I am viewing an article that has code examples in it, the formatting is always chancy at best. Sometimes the code displays great. Other times it is truncated or not present at all. The only way I can be sure I am seeing the full article is to open it in a browser. 

In Read Later, I was simply able to hit the Return **(↩)** key and the selected article would open in my default browser. This shortcut is not present in ReadKit. You can either hit **V** to view the original article within ReadKit, or hit **B** to view the article in its current form in your default browser. The problem here is that if you are currently viewing the Instapaper version of the article, it opens on Instapaper’s website in its stripped down format rather than opening to the native URL. So I would have to hit **V** then **B** on every article that I wanted to view in Safari.

I know this is a small problem and just one extra keystroke, but it was annoying me. So, I created a new Keyboard Maestro group that is only active from within ReadKit. From there, whenever I hit **⌘ + ↩**, it opens the article right in the default browser. 

Irritation solved! Below is a screenshot of the macro in case you are interested. It is very simple, but it fixed my issue.

![KM Macro](/images/post_images/2013/01/readkit_km_open_in_browser.png)

{% img fancybox /images/post_images/2013/01/readkit_km_open_in_browser.png%}


