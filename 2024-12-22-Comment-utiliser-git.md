---
layout: page
title: Comment utiliser Git
subtitle: Petit tutoriel sur l'utilisation du source control
cover-img: 
thumbnail-img:
share-img: 
tags: [git, hash-ak, server, sourcecontrol]
author: Hash-AK
---
Vous avez peut-etre deja entendu parler de "git" ou de "source control", probablement en rapport avec GitHub.  

Le _source control_ est un moyen de garder trace des versions d'un code sur lequel on travail. Cela permet de reveir en arriere si quelquechose tourne mal, de collaborer plus facilement, etc.  
[GitHub.com](https://github.com) est l'un des sites de source control les plus utiliser, mais dans note cas ici on va utiliser du source control _local_, avec git sur Linux. 
A noter que git a ete installer avec une approche comme celle mentioner [dans ce poste](https://www.geeksforgeeks.org/how-to-setup-git-server-on-ubuntu/).  

Dans ce poste je vous montrerai comment 1) utiliser git depuis la ligne de commande (CLI) sur Linux et 2) comment l'integrer a VSCode 


## Command Line

Tout d'abord il faut initialiser le repo git (la ou le code va etre archiver) sur le serveur distant.  
> [!NOTE]
> Cette section est plus informative car cette etape sera probablement deja faite pour vous

Si vous le pouvez, taper les commandes suivantes a la localisation specifier si vous avez acces au serveur, sinon aviser les personnes responsables.  
En tant que l'utilisateur 'git' :
git init --bare NomDuProjet.git  

Comme on peut le voir ici :  
![Prompt du git](/assets/img/Git-init-on-server.png)