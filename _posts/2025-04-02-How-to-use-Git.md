---
layout: post
title: How to Use Git
subtitle: A Quick Tutorial on Source Control Usage
cover-img: 
thumbnail-img:
share-img: 
tags: [git, hash-ak, server, sourcecontrol, linux, vscode, terminal, cli, code]
author: Hash-AK
---

You may have already heard of "Git" or "source control", likely in relation to GitHub.

_Source control_ is a way to track versions of code you're working on. It allows reverting changes if something goes wrong, facilitates collaboration, etc.  
[GitHub.com](https://github.com) is one of the most widely used source control platforms, but here we'll use _local_ source control with Git on Linux.  
Note that Git was installed using an approach similar to the one mentioned [in this article](https://www.geeksforgeeks.org/how-to-setup-git-server-on-ubuntu/).  

In this article, I'll show you how to 1) use Git from the Linux command line (CLI) and 2) integrate it with VSCode.

## Command Line

First, initialize the Git repository (where code will be archived) on the remote server.  

{: .box-note}
**Note:** This section is informational as this step will likely already be done for you.  
If possible, run the following commands at the specified location if you have server access, otherwise contact the responsible team.  

As the 'git' user:

git init --bare ProjectName.git

As shown here:  
![Git prompt](/assets/img/Git-init-on-server.png)

Next, on your local machine (where you code), simply type:

git clone git@(server-IP-or-hostname):/path/to/your/project.git

At this stage, the server will prompt for the Git user's password (unless your machine was pre-authorized). Contact administrators for the password.  
![Git password prompt](/assets/img/Git-requesting-password.png)  
If you just initialized the repo, the "warning: You appear to have cloned an empty repository" message is normal.  
Now navigate to the cloned directory (without the .git extension), add files, modify existing ones, etc.  
When ready to save this code version, run (within the directory):  

git add --all

Then:  

git commit -m "A brief message explaining the changes made"


Finally, when ready:  

git push


Note that the first time, Git will prompt you to configure global user.name and user.email. Follow the system's instructions (you can use any username@system.local format for email, as long as it's unique enough for later identification).

Now the code is saved on the server. You can repeat the process (clone, add, commit, push) from another network computer, enabling project collaboration.  
Note that on the server, in yourproject.git directory, you **WON'T SEE** your code directly. It's synchronized during each push but doesn't exist as regular files in the server directory. This is why you must git clone, add, commit, and push from another machine to work on your code.

![Git add, commit, push](/assets/img/Git-add-commit-push.png)  

Now let's move to VSCode.

## VSCode 
![Visual Studio Code](https://img.shields.io/badge/Visual%20Studio%20Code-0078d7.svg?style=for-the-badge&logo=visual-studio-code&logoColor=white)
, or VSCode, as Wikipedia states:  
> "An extensible code editor developed by Microsoft for Windows, Linux, and macOS.  
>  
> Features include debugging support, syntax highlighting, intelligent code completion (IntelliSense), snippets, code refactoring, and **built-in Git**. Users can customize themes, keyboard shortcuts, preferences, and install extensions that add additional functionality."  
> {..}  
> "In Stack Overflow's 2023 developer survey, Visual Studio Code was ranked the most popular development environment (IDE), with over 73% of 86,544 respondents reporting using it."

It's a feature-rich text editor with Git support, widely adopted. This section explains how to sync VSCode with a Git server on the same network.

{: .box-note}
**Note:** To use VSCode, your machine **MUST** have its SSH key authorized on the server. Confirm with your administrators.

First, click the three-dot icon connected by branches (as shown below):  
![VSCode source control button](/assets/img/vscode-sourcecontrol-button.png)  

This opens a top dialog. Enter the same command-line format (e.g., git@192.168.0.132:/usr/local/git/test.git). This will open your file explorer. Choose a destination folder.

VSCode will ask if you want to open the cloned repository - click 'Open':  
![Open cloned repo](/assets/img/vscode-open-cloned-repo.png)  
Finally, click 'Yes, I trust the authors' when prompted.  
Done! You should now see your repo files (if previously modified and pushed).

As you modify code or add files, you'll see a badge with numbers on the source control icon in the sidebar.  
This indicates unsynchronized local changes:  
![Unsynchronized changes badge](/assets/img/VScode-pastille-sourcecontrol.png)  
Let's synchronize:  
Click the source control button, then 'Commit changes'. A dialog will appear stating there are no _staged_ changes (VSCode hasn't run git add --all yet).  
Click "always".  
This opens a new file, likely .git/COMMIT_EDITMSG.  
Below lines starting with '#', write your commit message. Save the file (CTRL+S).  
A popup appears:  
![COMMIT_EDITMSG edit popup](/assets/img/VScode-editer-commit_editmsg.png)  
Click Save.  
You'll return to the source control view. Click the blue button with arrows in the sidebar:  
![VSCode push button](/assets/img/VSCode-boutton-push.png)  
Finally, click 'OK'.  

Note: A message may appear in the bottom-right asking if VSCode should periodically run 'git fetch'. This command updates your cloned repo if changes are pushed from other machines (e.g., collaborators). The choice to enable is yours.
