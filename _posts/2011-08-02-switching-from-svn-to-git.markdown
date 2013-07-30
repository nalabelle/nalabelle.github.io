---
author: admin
comments: true
date: 2011-08-02 00:51:41+00:00
layout: post
slug: switching-from-svn-to-git
title: Switching from SVN to Git
wordpress_id: 17
categories:
- Coding
---

Last week I came across [a post on Slashdot about Git](http://developers.slashdot.org/story/11/07/27/0012218/The-Rise-of-Git), stating that [Eclipse had picked up Git](http://www.infoworld.com/d/application-development/torvaldss-git-the-it-technology-software-version-control-167799?page=0,0) as a supported alternative to [Subversion](http://subversion.apache.org/). It was particularly interesting to me, as I had just been checking out alternatives to Subversion, like [Mercurial](http://mercurial.selenic.com/), even going so far as setting up the server and installing [Rhodecode](http://rhodecode.org/) to handle management. I figured that if [Eclipse](http://www.eclipse.org/) was supporting [Git](http://git-scm.com/) though, I should give it a try. I went through the quick, but painful process of setting up [Gitorious](http://gitorious.org/) on my server and started adding test repositories and seeing what it could do. I was pretty well impressed and decided to move to that platform, especially after seeing how much better [EGit](http://eclipse.org/egit/) worked than [MercurialEclipse](http://javaforge.com/project/HGE).

In SVN, I had set up a repository for each general area of coding I do, one for personal stuff, one for freelance work, and so on. In those, I had folders for each project. In the projects, I had folders for each version, basically a snapshot of the code at a landmark date. I looked and tried many different programs to convert them to git, but ultimately couldn't find anything that worked. I took an alternate route: set up an empty Git repository for each project, copied the versions in, tagged them, and copied the next highest version in. I lost a lot of my history, but thankfully that didn't really matter to me as long as I had the landmark versions.

It took me an entire day to copy and set up all my projects, but I've noticed myself committing more often with more descriptive comments and just pushing when I'm done, since it's so much faster and less troublesome than SVN. I'm definitely no power-user, but I never really was with SVN either. I also really like the idea of each clone being an entire repository in itself, in case something were to happen to my main repositories. Overall, I'm glad I chose to make the change.
