---
layout: post
title: (Makerspace's Server Part 4)
subtitle: Git, Samba and more!
cover-img:
thumbnail-img:
share-img:
tags: [markerspace, hash-ak, server, git, samba]
author: Hash-AK
---
## Git, Samba and more!
 
It's been a really long time since I posted here, so I will try to explain everythign that I did on the servers since.

As I said in my [last blog post](https://hash-ak.github.io/2025-01-29-Static-IP-on-the-server/), we had _git_ running on the server. You might ask "but what is git"? Git is a technology of distributed version control, which serves to keep versions of your previous attempts when you code, so that if your new patch/version doesn't work, you can 'go back in time' and start over at the previous git-saved version. It also allow to have different 'branchs' so you  can manage different versions (example the stable branch, the developpement branch, a testign branch with new patches, etc).

Accordign to [Wikipedia](https://en.wikipedia.org/wiki/Git) :

>Git is a distributed version control software system that is capable of managing versions of source code or data. It is often used to control source code by programmers who are developing software collaboratively.

>Design goals of Git include speed, data integrity, and support for distributed, non-linear workflows—thousands of parallel branches running on different computers.

>As with most other distributed version control systems, and unlike most client–server systems, Git maintains a local copy of the entire repository, also known as the "repo", with history and version-tracking abilities, independent of network access or a central server. A repository is stored on each computer in a standard directory with additional, hidden files to provide version control capabilities.

>Git provides features to synchronize changes between repositories that share history; for asynchronous collaboration, this extends to repositories on remote machines. Although all repositories (with the same history) are peers, developers often use a central server to host a repository to hold an integrated copy.

>Git is free and open-source software shared under the GPL-2.0-only license. 

So basically, hosting git would allow us to replace github, and have everything locally. In the context of a makerspace it can be really interesting. 

So I seted up git on the server, with these commands :
```bash
 sudo useradd –home /home/git –shell /bin/bash
 sudo mkdir /usr/local/git
 sudo chown git:git /usr/local/git
 sudo passwd git # choose the password you want, you will need it when doing git operations
```

