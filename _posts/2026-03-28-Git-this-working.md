---
layout: post
title: (Makerspace's Server Part 4)
subtitle: Git this working!
cover-img:
thumbnail-img:
share-img:
tags: [markerspace, hash-ak, server, git]
author: Hash-AK
---
## Git this working!
 
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
 sudo chown git:git /home/git
 sudo passwd git # choose the password you want, you will need it when doing git operations
```
/home/git then become the root of the Git server, so I can create a Git project while being logged in as the Git user with :
```bash
su git
cd /home/git
git init –bare projectname.git
```
I can then clone the repo from any hosts, with
```bash
git clone git@gitserver.local:/home/git/projectname.git
```
The only problem is that it will prompt for the password at every single git action. At first it didn't sound annoying by itself, but when I tried to make it work with VS Code's Git funcitonality and it didn't work, I realized I would have to use SSH Keys (for some reason VS Code always failed to clone the repo because the was just no password prompt popping up thus no authentification).
I added all the machines' public ssh keys to the /home/git/.ssh/authorized_keys file. For Linux hosts, it's siwas quite simple :
```bash
ssh-keygen -t ed25519
ssh-copy-id git@gitserver.local
```
For Windows (which was the major problem, where VS Code was), it was slightly more difficult :
```powershell
ssh-keygen -t ed25519
type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh git@gitserver.local "cat >> .ssh/authorized_keys"
```
I also had to install Git from [Winget](https://learn.microsoft.com/en-us/windows/package-manager/) like this :
```powershell
winget install --id Git.Git -e --source winget
```

One thing to note on both Windows and Linux, is that I also had to configure the local user's "git identity", with
```bash
git config --global user.name "Name"
git config --global user.email "user@machine"
```
Witouth that, the user can git clone a repo, but not push or anything that modifies the upstream.


