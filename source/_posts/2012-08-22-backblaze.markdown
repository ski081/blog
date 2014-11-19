---
layout: post
title: "BackBlaze, MDS Worker, and Spotlight"
date: 2011-08-08 22:56
categories: [Mac, Backblaze, Backup]
author: Mark Struzinski
comments: true
---

I recently began experiencing my system fans kicking in and running all the time. The fans began running so much that I almost didn&rsquo;t notice them any more. When I pulled the Activity Monitor up, the [md worker][mdworker] was using over 100% of my system resources consistently.

After looking up what md worker does, it appears to be a process spun off by spotlight to index your drive for quick searching. Even after a reboot, the fans would kick back in after a short idle time of maybe a minute. This process was seriously heating up the mac.

<!-- more -->

After doing some more research, I found this [excellent question and answer][superuserqa] from the [SuperUser][http://superuser.com/] site. I ran the following command in the terminal to log what Spotlight was trying on index at the time.

```bash fs_usage
sudo fs_usage -w -f filesys mdworker
```

The terminal showed the last file to be indexed was in the __/Library/BackBlaze/bzdata__ folder. This was not Library in __~/Users__, but the one at the base of the volume. It was indexing this file over and over. My initial thought was that the system was hung on the file. A google search turned up [this result][google_backblaze] on the BackBlaze support page. BackBlaze frequently writes log data to a file. All files in the BackBlaze folder should be added to the Privacy Exclusions so that Spotlight will not attempt to index them. According to the support article, this exclusion should be added when BackBlaze is installed. Since the Lion version for BackBlaze was very recently released, this may be a bug in the BackBlaze installer.

{% img /images/post_images/help-mac-spotlight.jpg %}

In my case, the folder had not been added.

After adding the BackBlaze folder to the Spotlight Privacy exclusions, the problem disappeared at once. Good job on BackBlaze for having an excellent support document.

UPDATE: I recently had to replace my hard drive and decided to do a fresh install rather than pulling over my [Carbon Copy Cloner][ccc] backup. I experienced the exact same problem, and had to follow the process outlined above to fix it. This issue hasn't been resolved in the BackBlaze installer as of yet.

[mdworker]:http://osxdaily.com/2009/09/14/mdworker-what-is-mdworker/
[superruserqa]:http://superuser.com/questions/46195/why-does-mds-run-wild-in-mac-os-x-10-6
[superuser]:http://superuser.com/
[google_backblaze]:http://www.backblaze.com/help-mac.html#filevault
[ccc]:http://www.bombich.com