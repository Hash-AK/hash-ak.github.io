---
layout: post
title: Updating the firmware
subtitle: Diving into UEFI problem on the makerspace's server
cover-img: 
thumbnail-img: 
share-img: 
tags: [makerspace, hash-ak, server, UEFI, BIOS, Firmware, Update, M72e, Lenovo, Thinkcentre]
author: Hash-AK
---
## Part 1: Starting the work!
So let's start this server!
As i said in [My previous blog post](/_posts/2024-11-24-Purpose-of-the-server.md), the v.1 was running on old laptops.  So the first step to start to the new devices ( _Lenovo Thinkcentre M72e Tiny_, to be precise) was to start from zero, well to get rid of the default Windows installed on them.


![Get rid of windows meme](https://i.imgflip.com/6bflpq.jpg)
_source: [imgflip.com](https://imgflip.com/i/6bflpq)_ 

Of course not that way. So we booted on a Ubuntu 18.04 USB stick, did the whole install process, formatted the disk, and lets go! What could go wrong? 
Welp. **Everything**
When we rebooted without the USB stick, it said "_No bootable device found_"
So the device didn't seems to have installed Linux. Or something got corrupted in-between. Or maybe it was the **UEFI/Legacy** detection.
So we looked back the the BIOS (American BIOS, by the way), tried changing boot order, boot mode (UEFI to Legacy, Legacy to UEFI). 
Nothing.

We tested on another device (Still a M72e Tiny)
And on that one detected correctly the UEFI mode and installed it just fine. What is the difference? Why did they reacted that differently?

Well, looking back at the BIOS, we realized something important : The BIOS version/date was different on the 2 devices, the second one, the one that booted correctly, being more recent (i cant remember the exact version but the one that didn't boot had a BIOS version of 2012 and the other one had a BIOS from 2013). I think we found our problem...



{: .box-note}
>**Information : Why is it important to keep BIOS firmware up-to-date?**
>"The BIOS (Basic Input/Output System) is a fundamental piece of firmware embedded in a computer. It’s responsible for booting up the system and managing data flow between the computer’s operating system and attached devices. Over the years, BIOS has evolved from basic firmware to a more complex and integral part of computer functionality, adapting to the increasing sophistication of modern hardware and software.
>
>Keeping one’s BIOS updated is crucial for ensuring that a computer’s hardware is fully compatible with its operating system. These updates can enhance system performance, improve hardware compatibility, fix bugs, and provide security patches. They are essential for maintaining the overall health and efficiency of a computer.
>BIOS updates refine the firmware that controls hardware-software interaction. They are **essential for adapting the system to new technological standards** and often include advanced features like secure boot in modern UEFI (Unified Extensible Firmware Interface)"  
> _source: [Ninjaone.com](https://www.ninjaone.com/blog/how-to-update-your-pcs-bios/)_

## Part 2: We've found the BIOS (And We're Mad After Windows..)

A [quick Google search](https://support.lenovo.com/us/en/downloads/ds029184-flash-bios-update-thinkcentre-m72e-tiny) showed that the latest BIOS update was from 2018. So our machines where not at the latest BIOS, and even worse : they where not all at the same update.
But there's a little problem : primo, on the first machine, there was no more OS (Windows being wiped-out and Linux not installed) and segundo there's no Linux in the "**_Compatible Operating Systems_**" section, as i show below :  
>## Compatible Operating Systems
>Windows 7 32bit, 64bit  
>Windows 8 32bit, 64bit  
>Windows 8.1 32bit, 64bit  
>Windows Vista 32bit, 64bit  
>Windows XP 32bit, 64bit
_source : [support.lenovo.com](https://support.lenovo.com/us/en/downloads/ds029184-flash-bios-update-thinkcentre-m72e-tiny)_

Erm, only (MS-)DOS-based systems, or simply, Windows.
We still tried to boot on [the given .ISO](https://download.lenovo.com/pccbbs/thfinkcentre_bios/f4j961usa.iso) but it didn't seems to boot (we saw its name in the boot manager but it just rebooted the computer without starting the installation). 
Will we be forced to reinstall Windows? 
The answer is : **_Nope_**  
Thanks to [FreeDOS](https://www.freedos.org/).

{: .box-note}
>**What is FreeDOS?:** According to their website,
>"FreeDOS is an open source DOS-compatible operating system that you can use to play classic DOS games, run legacy business software, or write new DOS programs. Any program that works on MS-DOS should also run on FreeDOS."
> _Source: [FreeDOS.org](https://www.freedos.org/)

So basically you can just boot on that system from a live USB stick, and run .exe files! 
## Part 3:  How to update the BIOS, step by step
So we're not doomed to use windows again. Oof...
With that card in hand its actually pretty easy to update the firmware : grab [the zip](https://download.lenovo.com/pccbbs/thinkcentre_bios/f4jt61usa.zip)for the M72e Tiny's latest BIOS, grab FreeDOS's [LiteUSB](https://www.ibiblio.org/pub/micro/pc-stuff/freedos/files/distributions/1.3/official/FD13-LiteUSB.zip). Put that in a folder, whatever it is/ Plug an USB stick (formatted) in your Linux, make sure its unmounted. First, unzip the FreeDOS zip, then after checking what device is your USB key (with lsblk), do (_note_: **replace sdb with your USB stick's device name)**: 
```bash
cat FD13LITE.img > /dev/sdb
```
After that, mount the key with the GUI. You should see the FreeDOS files correctly setup in the folder (probably mounted in /media/(your username)/(device name) if you're on Ubuntu). **_If it didn't work_**, you can still try to use a flashing tool like Etcher. 

Next, create a directory inside the FreeDOS key. Call it "_lenovo_". Unzip the firmware from the Lenovo BIOS update inside the "_lenovo_" dir, remove the USB stick (don't forget to unmount first!) and here you go!

You just plug it in the device, open the BIOS, take note of the existing BIOS version, load back the default settings,reboot boot on the USB stick (if you have the option in the boot menu choose Legacy, else just select the USB's name in the list).
Select 'English' as language, and click on "No, Return to DOS" when prompted if you want to install FreeDOS :  
![Loading default settings in the BIOS](/assets/img/Load-defaults-BIOS.png)  
_Loading defaults settings in the BIOS_  

![Selecting the correct language in FreeDOS](/assets/img/FreeDOS-select-language.png)  
_Selecting the correct language in FreeDOS_

![Coming back to FreeDOS and aborting the install process](/assets/img/FreeDOS-return-to-dos.png)  
_Coming back to FreeDOS and aborting the install process_

The command prompt will then open. Just type the following :
```
cd lenovo\
autoexec.bat
```
![FreeDOS command prompt](/assets/img/FreeDOS-command-prompt.png)  
_FreeDOS command prompt with the command_  

Here we go!
Then say 'No' to both "Updating Serial Number" and 'Updating Machine Number' prompt.  
![Refusing to update the Serial Number](/assets/img/BIOS-updating-SN.png)  
![Refusing to update the Machine Type](/assets/img/BIOS-updating-MachineType.png)  
_Say 'No' to both of the prompts_  

Then, you just need to wait ^^ 
It will reboot the computer.  
![Image of the process bar](/assets/img/BIOS-update-process2.png)

{: .box-warning}
**Warning:** If you see that its stuck powered on but with no signal on the display for **MORE** than five minutes, remove the USB stick and press the power button. It should wake it up and finish the tasks.  

![Image of the process after the system reboot](/assets/img/BIOS-update-after-reboot.png)  
_Image of the process after the system reboot_

Next when its finished remove the USB stick, if you didn't already, and you should see the BIOS version updated! 
Here are some pictures to show the steps in the process :

![The BIOS version is up-to-date now!](/assets/img/BIOS-update-complete.png)  
Then I could finally install Ubuntu 18.04 to test if it was ok.  I started the install process of Ubuntu 22.04 Live Server on one of the machines (after the hard drive crashed during install so I had to restart the process).
There's still a loooong road  before it becomes a fully functional server. 

So here's all for today! Stay tuned for the continuation of this project!

