---
layout: post
title: (Makerspace's Server Part 2)
subtitle: Always Check your installation medias!
cover-img: 
thumbnail-img: 
share-img: 
tags: [makerspace, hash-ak, server, media, usb, installation, M72e, Lenovo, Thinkcentre, Ubuntu, Ubuntu 22.04]
author: Hash-AK
---
## Always Check Your installation medias! 

Today's post is pretty short lol. It's a friendly reminder to **always** check your installation medias. Why? Let me explain the story :



After [last time's BIOS update](/2024-11-25-Updating-the-firmware), we thought we could just install Ubuntu 22.04 Desktop on one of the machines. As we plug the USB and open the boot menu, something weird happens : the USB key installation media (that was flashed a bit of time ago) wasn't recognized as Legacy/UEFI in the boot menu. 

We started panicking, as we though _maybe_ the BIOS update didn't fix all the boot problems. WE tried booting on it, but it didn't even work. On another computer (DELL Laptop)  Ubuntu booted fine, but not on the M72e. 

We then tried reflashing the .iso on the same USB key with the following command :
```bash
pv -peartbW Ubuntu-22-04-Desktop.iso > /dev/sdb
```

And then, we saw an error message : "Not enough place left on the device"
After verification, we found that the USB was nearly 4GB and the .iso was.... 5GB. 
So after taking a bigger SB and reflashing,, Ubuntu booted and installed just fine.


So yeah! Always check your Installation medias!  
**_Hash-AK_**
