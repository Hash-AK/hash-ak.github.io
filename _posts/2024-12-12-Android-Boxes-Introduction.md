---
layout: post
title: (Android TV Boxes 1)
subtitle: The Amlogic Fever
cover-img: 
thumbnail-img: /assets/img/Android-Boxes-2-images.jpg
share-img: 
tags: [android, hash-ak, linux, amlogic, kernel, uart, ttl. s805, s802, mxq. mxiii. libreelec, armbian]
author: Hash-AK
---


Today I'm starting a new 'serie' of post, along of the server, but for another Linux project. I have two Android TV box, running version 4.4.2. One is branded under the name "Polar", with an Amlogic S805 CPU and board name MXQ in android settings, and the other one is branded under the name 'Polar 4K Arctic', Amlogic s802 processor and name MXiii in the settings. 

As they have a really old Android version, I wanted to install Linux and use them for stuff (example a NAS or just some experimentation machines). Sadly, after my research, nearly all the links for downloading the image (mainly from Armbian's forum, a port of Linux for ARM devices) are dead/deleted.

Then I found out [LibreELEC images working on both my S805 and S802](https://github.com/dtechsrv/LibreELEC-AML/releases), thanks to the incredible port of [dtechsrv](https://github.com/dtechsrv)on GitHub. I was able to fully verify my device's specs (and to get some motivation!). It was one of the only Linux images that aren't deleted for aml s802/s805 boxes.

>LibreELEC (short for Libre Embedded Linux Entertainment Center) is a non-profit fork of OpenELEC as an open source software appliance, a Linux-based Just enough operating system for the Kodi media player. This fork of OpenELEC announced in March 2016 as a split from the OpenELEC team after "creative differences", taking most of its active developers at the time to join the new LibreELEC project.
_Source: [Wikipedia](Https://En.Wikipedia.Org/Wiki/Libreelec "Wikipedia")_
-----------------------------------------------------------------------

## UART Mode Enabled


After finding [a tutorial explaining how to use UART on an S802 device](https://www.cnx-software.com/2014/05/07/how-to-open-tronsmart-vega-s89-elite-and-access-the-serial-console/), I bought an USB-to-TTL converter. After I plugged it in the pin on my S802 device (Remember, the 'Arctic 4K' ).

I was able to see the boot logs! But then I found out I can interrupt the boot process to get a shell. Here's a few info I got from it :

```console
root@k200_MXIII_1G:/data # uname -a
Linux localhost 3.10.33 #1 SMP PREEMPT Sat Aug 15 11:45:15 CST 2015 armv7l GNU/Linux

root@k200_MXIII_1G:/data # free -m                                             
             total         used         free       shared      buffers
Mem:          1599          724          874            0           22
-/+ buffers:                702          896
Swap:          499            0          499

```

Weirdly enough when plugged on my S805 board I get no boot logs, so I wasn't able to test on that one sadly.

But now I have more information :
- One of the box is a mxiii S802 1GB of ram, with the k200 name.
- It uses kernel 3.10.33, from around 2010-2015
- The other one is a MXQ (HD18Q?) S805, 1GB too. 
- Both have booted with LibreELEC (S805 was [this one](http:/https://libreelec.dtech.hu/images/S805/LibreELEC-HD18Q.arm-9.2.8.15.img.gz/ "this one"), and S802 with [that one](http://https://libreelec.dtech.hu/images/3rdParty/LibreELEC-MXIII-1G.arm-9.2.8.15.img.gz "that one"))
- There was a port of Linux on them (again, LibreELEC). built with kernel 3.10, again. Saddly LibreELEC in itself lack the functionalities I need (namely a package manager, gcc/make, optionally a GUI)

I still have a some work before reaching my goal ^^`
