---
author: admin
comments: true
date: 2009-06-16 14:11:19+00:00
layout: post
slug: wordpress-sftp
title: WordPress SFTP
wordpress_id: 64
categories:
- IT
tags:
- Hardy
- PHP
- Server
- SFTP
- Ubuntu
- WordPress
---

Note: This post has been superseded by a newer post. [Read it here](http://hypertoast.net/blog/2010/06/10/wordpress-sftp-part-2/).

I don't like to use FTP at all when I can help it. It's an old protocol and doesn't hold up in regards to security. I could use FTPS, which is a Secure Sockets Layer over FTP, requiring a key exchange, but frankly, that sounds like too much work for me. Since I already use SSH and SSH has file transfer capability, I usually use that to manage things. It's commonly called SFTP. Unfortunately, this breaks many apps that expect you to be using FTP, such as WordPress.

Little known to me was that this wasn't the fault of WordPress; those glorious developers had already added ssh file transfer support to it some time ago. What was lacking was communication between PHP and SSH. When I researched online, I found instructions on how to get them talking to each other, which kicks in the SFTP support in WordPress. I'll detail them here for reference. (Note, this is on Ubuntu Hardy, and I'm using Apache 2 and PHP5.)

    
    user@computer:~$ sudo apt-get install libssh2-1-dev php-pear
    user@computer:~$ sudo pecl install -f ssh2
    user@computer:~$ sudo nano /etc/php5/apache2/php.ini


Find the section titled "Dynamic Extensions" and below, add:

    
    extension=ssh2.so


Save it and exit, and then just do an Apache restart.

thanks to [[Kevin van Zonneveld](http://kevin.vanzonneveld.net/techblog/article/make_ssh_connections_with_php/)] 
