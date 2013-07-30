---
author: admin
comments: true
date: 2011-07-11 18:55:43+00:00
layout: post
slug: wordpress-e-commerce-store
title: Wordpress e-Commerce Store
wordpress_id: 12
categories:
- Web
---

I've recently been working on store sites for a client. This client asked me to work on another store for him, and during my initial data-gathering process, I suggested that we give [WordPress](http://wordpress.org/) and an e-commerce plugin a try. I prefer WordPress' simplicity over the complicated mess that [Joomla](http://www.joomla.org/) is, but granted, every software has a niche where it can shine.

Anyway, I took a look through the available plugins for e-commerce and settled on the widely used [WordPress e-Commerce](http://www.instinct.co.nz/e-commerce/). I installed it, set up a few test categories and items, and handed an admin password over for the client to give it a try and see if it was agreeable. As it turned out, there was a problem in the plugin where you couldn't switch the editor from HTML to Visual or vice versa. Loaded up [Firebug](http://getfirebug.com/) and found it wasn't properly including a certain JavaScript file. I also noticed that it wasn't properly uploading photos, and not applying category photos. Digging through the logs, I found it was trying to write to a not created directory, ./wp-content/uploads/wpsc. Created the directory, changed some permissions, and reinstalled the plugin. Everything was good again and the client was happy with the choice.

Exciting? Not really. I'm just glad it was an easily fixable problem, but I'm still not exactly sure why the JavaScript was failing when there isn't any in the directories it created.
