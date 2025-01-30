---
layout: post
title: (Makerspace's Server Part 3)
subtitle: Static IP on the server (and more)
cover-img: 
thumbnail-img: 
share-img: 
tags: [hash-ak, linux, ip, static ip. server, makerspace]
author: Hash-AK
---
Hello! It's been a long break since my [last post about the makerspace's server](https://hash-ak.github.io/2024-12-02-Always-Check-Your-Installation-Medias/). I will try to correct that today.

## Why (and what is) a static IP ?

As you may know, each time you're on a network, you're assigned an _internal_ IP address. Most of the time, on residential networks, it's something in the line of "192.168.x.xxx". The problem is, each time you power up your device, there's a chance you get a random ip from these possibility (meaning that the IP won't always be the same).  
Where's the problem, you may ask ? Well, a server need, _most of the time_, the less possible human interactions, thus not relying on a display. If you don't use a display, you need to remote-connecte (ether with RDP, VNC, SSH, and the like).
The main problem is : **if you get a random IP, how will you remote login?** You can't just rely on the random-factor of the IP, as sometimes you will get the same IP, but if your router restart, or sometimes just randomly, you will just get a different IP.

What's the solution, then?

_Static IPs_

Basically, you simply change a configuration on the server that _asks_ the router (or the DHCP) to assign a specific IP from the available range, for example 192.168.1.337. That means you can just keep the same IP for remote logins!

Seems cool, huh?

Well it was _sort of_ harder than expected. I follow [this tutorial](https://www.freecodecamp.org/news/setting-a-static-ip-in-ubuntu-linux-ip-address-tutorial/), everything **_seemed_** to work fine, like the IP was the one I entered, I could ssh from the local network to the correct IP.

Great. Then came the trouble.


## In troubles


As I was over ssh, I tried a simple 'apt install' because something was missing (like ViM or something like that). It said it coudln't fetch the Ubuntu's apt website. I tried to ping google. No luck, no connection. But I was still over ssh! From the computer whith which I was ssh-ing (so my client, not the server), I could reach google just fine.
What was the problem?

- Not my network, as I could reach google from a different machine.

- Not the server being disconnected of the local network, because I could ssh.

- Not the apt website or google being down (duh)

The problem was the Netplan configuration (the thing that control the static IP).
I moved the custom config to config.bak, reseted back to the default then reloaded netplan. It worked fine (of course, with a random IP, because _my custom static IP configuration is gone_)
After a lot of researchs, the use of [perplexity.ai](https://www.perplexity.ai/), and a few configuration checks, I found out the problem.

The traceroute (the route your computer need to take to reach the Net) was wrong, meaning that _inside_ the network, everything was fine, I could ping, ssh, RDP, etc the _local_ machines, but outside it (google, apt repo, github, etc) was simply _unreachable_ for the server with static IP.

I then corrected that mistake and launched back the netplan rule, so the static IP worked _and the connection to apt too!_


Now, you literally just need to plug the server to ethernet/wifi, connect the router (if it's not already connected), and it all setup by itself! No need of a display before ssh-ing.

In thew next post, I will probably try to catch-up on the git tentatives (because yes, we have git on it!)

That's one step forward to our goal. See you soon for other server-related stories!  
**_Hash-AK_**