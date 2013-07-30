---
author: admin
comments: true
date: 2010-02-22 18:30:03+00:00
layout: post
slug: gtalk-highlight-notification-for-xchat
title: Gtalk Highlight Notification for XChat
wordpress_id: 144
categories:
- Coding
tags:
- Chat
- google talk
- IRC
- notification script
- xchat
---

I'm a moderator (admin? op?) on a very small IRC room. I always have at least one client running somewhere, usually on my Linux computer. Naturally I use [XChat](http://xchat.org/). Invariably someone will attempt to contact me via IRC when I'm not around, so I whipped together a quick little XChat plugin script that'll send myself a [Google Talk](http://www.google.com/talk/) message when someone writes a highlighted phrase, such as my nickname. If I'm not busy, I can then pop on and see what they wanted. Hopefully it won't be too annoying, I can always just turn it off (I know at least one of you reads this. I will most definitely ban you if you bother me at the wrong time ;) ).

Anyway, click the "read the rest of this entry" link for the script if you want to look at it.

<!-- more -->

    
    #!/usr/bin/perl -w
    #
    # Gtalk Highlight Notification
    #
    # Simple script that sends a GTalk Notification upon a highlighted word
    # by almiteycow
    #
    #
    # ---------------------------------------------------------------
    # Jabber/GTalk implementation from
    # http://www.pervasive-network.org/SPIP/Google-Talk-with-perl-bis
    #
    # by Thus0
    # Copyright (c) 2005, Thus0. All rights reserved.
    #
    # released under the terms of the GNU General Public License v2
    
    use strict;
    use warnings;
    use Xchat qw( :all );
    use Net::XMPP;
    
    ## Configuration
    ## Put the account you'd like to send notifications FROM here. You might need to make a second account.
    our $googleusername = "username";
    our $googlepassword = "password";
    ## Put the account you'd like to send notifications TO here.
    our $to = "notificationuser";
    
    our $body;
    our $resource = "PerlBot";
    
    ## End of configuration
    
    #------------------------------------
    
    # Google Talk & Jabber parameters :
    
    our $hostname = 'talk.google.com';
    our $port = 5222;
    our $componentname = 'gmail.com';
    our $connectiontype = 'tcpip';
    our $tls = 1;
    
    #------------------------------------
    
    my $name = 'Notification Script';
    my $version = '0.1';
    our $state = 0;
    
    register($name, $version, 'Notification Script');
    prnt("Loaded $name $version");
    
    foreach ('Channel Msg Hilight') {
    	hook_print($_, &connectAndSend);
    }
    hook_command('notify', &notifytoggle, { help_message => 'Usage: /notify (0/1) to turn off/on notfications' });
    
    sub notifytoggle {
    	if ($_[0][1] == "1") {
    		$state = 1;
    		Xchat::print("GTalk Notification On");
    	} elsif($_[0][1] == "0") {
    		$state = 0;
    		Xchat::print("GTalk Notification Off");
    	} else {
    		Xchat::print("invalid option");
    	}
    	return EAT_XCHAT;
    }
    
    sub connectAndSend {
    	if($state == 1) {
    		my $highlighted = $_[0][0];
    		my $msg = $_[0][1];
    		my $data = "@{$_[0]}";
    		my $ircserver = Xchat::get_info("server");;
    		my $ircroom = Xchat::get_info("channel");
    		$body = "New Message from $highlighted: $msg on $ircserver$ircroom";
    
    		my $Connection = new Net::XMPP::Client();
    
    		# Connect to talk.google.com
    		my $status = $Connection->Connect(
    			hostname => $hostname, port => $port,
    			componentname => $componentname,
    			connectiontype => $connectiontype, tls => $tls);
    
    		if (!(defined($status))) {
    			prnt "ERROR:  XMPP connection failed.n";
    			prnt "        ($!)n";
    			exit(0);
    		}
    
    		# Change hostname
    		my $sid = $Connection->{SESSION}->{id};
    		$Connection->{STREAM}->{SIDS}->{$sid}->{hostname} = $componentname;
    
    		# Authenticate
    		my @result = $Connection->AuthSend(
    			  username => $googleusername, password => $googlepassword,
    			  resource => $resource);
    
    		if ($result[0] ne "ok") {
    			prnt "ERROR: Authorization failed: $result[0] - $result[1]n";
    			exit(0);
    		}
    
    		# Send message
    		$Connection->MessageSend(
    			to => "$to@$componentname", body => $body,
    			resource => $resource);
    	}
    
    	return EAT_NONE;
    }
