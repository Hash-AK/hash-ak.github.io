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

Ensuite, sur votre machine locale (celle ou vous codez) tapez simplement :
git clone git@(ip ou nom local du serveur):/chemin/vers/votre/projet.git  
Normallement, a cette etape si le serveur vous demandera le mot de passe pour l'utilisateur git (a moin que votre machine ai ete authorisee a se connecter au prealable). Communiquer avec les administrateurs pour le mot de passe
![Git demandant le mot de passe](/assets/img/Git-requesting-password.png)  
Si vous venez juste d'init le repo c'est normal que le systeme dise 'warning: You appear to have cloned an empty repository.'
Maintenant vous pouvez allez dans le dossier avec 'cd (nom du dossier cloner, sans le .git), rajouter des fichier, modifier des fichiers existants, etc.
Quand vous etes pret a garder cette version du code, taper (dans le dossier) :  
git add --all  
ensuite:  
git commit -m "un bref message qui explque les changements apportees"
et enfin, quand vous etes prets :
git push  

A noter que la premiere fois le code va vous demander te vous identifier avec git --global user.name etc. Faites comme le system vous le demande (vous pouvez entrer n'importe quoi comme utilisateur@systeme.loc pour l'email et le nom d'utilisateur, tant que c'est assez unique pour que vous vous reconaisiez apres)

Maintenant le code est sauvegarder sur le serveur. Vous pouvez faire le meme processus (clone, add, commit, push) depuis un autre ordinateurs du reseau, ce qui permet de la collaboration sur un projet. 
A noter que sur le serveur, dans le dossier votreprojet.git vous **ne verrez PAS** votre code. Il est syncroniser a chaque push mais il n'existe pas en tant que tel dans le dossier sur le serveur. C'est pourquoi pour travailler sur votre code vous devez le git clone, add, commit, push comme mentionner sur une autre machine.

![Git add, commit, push](/assets/img/Git-add-commit-push.png)  
