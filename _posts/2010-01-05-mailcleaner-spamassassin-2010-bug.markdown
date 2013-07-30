---
author: admin
comments: true
date: 2010-01-05 17:16:48+00:00
layout: post
slug: mailcleaner-spamassassin-2010-bug
title: Mailcleaner SpamAssassin 2010 Bug
wordpress_id: 139
categories:
- IT
tags:
- mailcleaner
- Server
- SMTP
- spam filter
---

SpamAssassin was reported to have a date-related bug in it, you can [find more info here](http://it.slashdot.org/story/10/01/02/0027207/SpamAssassin-2010-Bug). This post will detail the way you can fix the bug if you happen to be running [mailcleaner](http://www.mailcleaner.org/). I haven't tested it fully, and don't know how permanent it'll be, but hopefully it'll be of some use.

<!-- more -->

First, open an ssh connection to your mailcleaner server. I'm using Linux, if you're on Windows, use [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/) or something.:

    
    ssh root@mailcleaner


The default password is "def". Then, do:

    
    nano /usr/local/share/spamassassin/72_active.cf


A quick little text editor should pop up. Hit Ctrl+W to do a search, and type "FH_DATE_PAST_20XX" into the search field. Hit enter. You'll see a line like the following:

    
    header   FH_DATE_PAST_20XX      Date =~ /20[1-9][0-9]/ [if-unset: 2006]
    


Change it to:

    
    header   FH_DATE_PAST_20XX      Date =~ /20[2-9][0-9]/ [if-unset: 2006]


Ctrl+X to exit, and Y, when asked if you want to save. You're mostly done. You just need to log into the admin interface and restart the applicable services. Or reboot, if you don't know how to do that.

The part I'm not too sure about is when the rule will be overwritten. I haven't investigated to see if mailcleaner is grabbing a new set of rules every so often or not, so I'll just have to keep my eye on the scoring for now.
