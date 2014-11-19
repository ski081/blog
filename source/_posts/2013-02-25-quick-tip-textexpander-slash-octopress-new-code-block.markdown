---
layout: post
title: "Quick Tip: TextExpander/Octopress New Code Block"
date: 2013-02-25 13:21
comments: true
categories: [Quick Tips, Octopress]
author: Mark Struzinski
---

Just a quick tip. This blog is built using [Octopress][octopress]. Octopress uses [Jekkyl][github] to create a static HTML site using markdown files for the content. I’ve been slowly building up some markdown specific [TextExpander][smilesoftware] snippets and [Keyboard Maestro][keyboardmaestro] macros to make things easier.

<!-- more -->

I got some great Keyboard Maestro tips from Patrick Welker in [this RocketInk article][km guide]. I’ve also been creating small TextExpander snippets make it easier to put in blocks of code and other commonly used elements.

Here is one I recently created that I use a lot. When I type **__ocb**, TextExpander creates a code block and places the cursor in the center and ready to accept input. It also asks for the language in a drop down menu (I’ve only included language selections as needed), and a title for the code block. This adds the correct text in the initial line of the code block that will add syntax coloring and a title to the top of the code block. 

So, this (minus the underscores):

__``` objective-c Test Code Block

NSLog(@“Testing”);

__```


turns into this:
``` objective-c Test code block

NSLog(@“Testing”);

```

[Here is a link][cl] to the snippet. 

Let me know if you have any feedback/questions.

[km guide]:http://rocketink.net/2013/01/markdown-maestro-guide.html (Ultimate Markdown Maestro Guide — RocketINK)
[cl]: http://cl.ly/180X3l3e0y32 (CloudApp link)
[github]: https://github.com/mojombo/jekyll (Jekyll static blog generator)
[keyboardmaestro]: http://www.keyboardmaestro.com/ (Keyboard Maestro)
[octopress]: http://octopress.org/ (Octopress - a blogging framework for hackers)
[smilesoftware]: http://www.smilesoftware.com/TextExpander/index.html (TextExpander home page)