---
author: admin
comments: true
date: 2009-06-20 01:21:40+00:00
layout: post
slug: ich9r-raid-with-windows-7-or-vista
title: ICH9R RAID with Windows 7 or Vista
wordpress_id: 82
categories:
- IT
tags:
- hard drive
- intel
- sata drivers
- western digital
---

This problem starts with the beta release of Windows 7. I was using XP, going along fine, but figured 7 looked great, and hey why not give it a try. Little did I know the huge headache this would cause me. Twice.

So, let's start with specs related to this problem, for those of you who are looking for help for similar problems:

    
    Abit IP35 Motherboard
    ICH9R SATA controller
    Western Digital Raptor Drives x2


Now, this may help people with a various range of equipment. I think it applies to a good portion of the Intel ICH series SATA controllers, and I bet it's not just limited to my motherboard or hard drives. Leave a comment if it this helps you, so other people searching can find the answer as well. Read on for more information.

<!-- more -->

Now, it started when I tried to upgrade to 7. I backed up all my data and shut down. I had come across another WD Raptor in my travels, so I figured, why not use the RAID ability of my motherboard to add a striped RAID0 for the system disk. Installed them both, booted, set up the RAID in BIOS, and then booted up the disc and started the install. I was pleasantly surprised to see that Microsoft had added the SATA drivers for my motherboard, so the RAID partition showed up promptly and correctly. I followed the prompts and let it install; everything went swimmingly, it was the quickest easiest Windows install I'd ever done. It went to go do the first of many reboots; I watched and waited. It continued setting up after the reboot, now running off the hard drive and doing the "configuring your system" part of the install. It was very very slow, but I was patient. Then all of a sudden, my system rebooted. That was OK, I figured, it was just doing another reboot for its setup. I was entirely wrong. Upon reboot it told me that something had gone wrong with the install and I would have to do it over.

After many reboots and configuration changes, I realized that it seemed to work fine if I had my motherboard in IDE or ACHI mode. IDE wasn't an option, as I wanted the speed benefits. I was persistent, so AHCI wouldn't cut it either. I tried many different versions of the drivers for the ICH9R: Intel's, Abit's, Microsofts. I tried loading them during install, loading them after I had it installed in AHCI mode. Nothing worked. Eventually I figured out that if I used a spare SATA drive I had laying about, I could install it on there and clone the drive back onto the RAID drives. I did so, and everything was happy, for a while.

Eventually, Windows 7 RC came out, and I would be forced to upgrade from Beta to RC starting July. I downloaded the new disc and burned it to DVD. Backed up my data, rebooted and started the process. I had a hint of remembrance for the last time, but I figured it was an update and a problem they had already noticed and fixed. Wrong again. Same problem happened. I forgot most of the process I did last time to make it work as well. I did try the other drive thing eventually even. Still nothing. I came around to noticing that the driver that was installed said "Intel(R) ICH9R/DO/DH SATA AHCI Controller". I looked at the Intel Matrix Storage Console and nothing RAID related showed up. I figured there was something wrong there. So, I decided to try to force it into loading the RAID controller driver instead. To my amazement, it worked.

So, here's the way to install Windows 7 or Vista on a computer with RAID on these components. Take note, you could definitely potentially screw up your computer and cause irreparable harm to it or yourself by following these directions. I take no responsibility.



	
  1. Grab the latest Intel Matrix Storage software from their site and extract it, or grab the floppy drivers for the install, doesn't really matter. Extract them wherever, maybe on a usb drive.

	
  2. Install your OS onto one of the drives in AHCI mode, if it won't install successfully, you'll need to find a spare SATA drive and do it to that drive.

	
  3. Boot into the OS and install the drivers, if you haven't done it during the install.

	
  4. You'll notice the above Intel ICH9R SATA **ACHI **driver in device manager.

	
  5. Launch regedit, and find these keys:
HKEY_LOCAL_MACHINESYSTEMCurrentControlSetservicesiaStor
HKEY_LOCAL_MACHINESYSTEMCurrentControlSetservicesiaStorV
HKEY_LOCAL_MACHINESYSTEMCurrentControlSetservicesmsahci

	
  6. You need to modify the "Start" parameter of them, mine are set to the following: iaStor is set to 3, iaStorV is set to 0, msahci is set to 0.

	
  7. Reboot, and change your BIOS setting to RAID before Windows starts.

	
  8. You should now check device manager to see if it says "Intel(R) ICH8R/ICH9R/ICH10R/DO SATA** RAID **Controller".
[![](http://hypertoast.net/blog/wp-content/uploads/2009/06/deviceman-269x300.png)](http://hypertoast.net/blog/wp-content/uploads/2009/06/deviceman.png)

	
  9. You can now clone your drive onto your RAID install, or just expand your drive into a RAID using the Intel Matrix Storage Manager program. I did the clone and it worked.

	
  10. _Enjoy!_


