---
layout: post
title: Comment utiliser Git
subtitle: Petit tutoriel sur l'utilisation du source control
cover-img: 
thumbnail-img:
share-img: 
tags: [git, hash-ak, server, sourcecontrol, linux, vscode, terminal, cli, code]
author: Hash-AK
---

Vous avez peut-être déjà entendu parler de "Git" ou de "source control", probablement en rapport avec GitHub.

Le _source control_ est un moyen de garder trâce des versions d'un code sur lequel on travail. Cela permet de reveir en arrière si quelque-chose tourne mal, de collaborer plus facilement, etc.  
[GitHub.com](https://github.com) est l'un des sites de source control les plus utilisés, mais dans notre cas ici on va utiliser du source control _local_, avec git sur Linux. 
À noter que git a été installé avec une approche comme celle mentionnée [dans cet article](https://www.geeksforgeeks.org/how-to-setup-git-server-on-ubuntu/).  

Dans l'article présent je vous montrerai comment 1) utiliser git depuis la ligne de commande (CLI) sur Linux et 2) comment l'intégrer a VSCode 


## Command Line

Tout d'abord il faut initialiser le repo git (là où le code va être archivé) sur le serveur distant.  

{: .box-note}
Note: Cette section est plus informative car cette étape sera probablement déja faite pour vous
Si vous le pouvez, taper les commandes suivantes à la localisation spécifier si vous avez accès au serveur, sinon aviser les personnes responsables.  

En tant que l'utilisateur 'git' :
```console
git init --bare NomDuProjet.git  
```
Comme on peut le voir ici :  
![Prompt du git](/assets/img/Git-init-on-server.png)

Ensuite, sur votre machine locale (celle où vous codez) tapez simplement :
git clone git@(ip ou nom local du serveur):/chemin/vers/votre/projet.git  
Normallement, à cette étape si le serveur vous demandera le mot de passe pour l'utilisateur git (à moin que votre machine ai été authorisée à se connecter au préalable). Communiquer avec les administrateurs pour le mot de passe
![Git demandant le mot de passe](/assets/img/Git-requesting-password.png)  
Si vous venez juste d'init le repo c'est normal que le système dise 'warning: You appear to have cloned an empty repository.'
Maintenant vous pouvez allez dans le dossier avec 'cd (nom du dossier cloner, sans le .git), rajouter des fichiers, modifier des fichiers existants, etc.
Quand vous êtes prêt à garder cette version du code, taper (dans le dossier) :  
```console
git add --all
```
ensuite:  
```console
git commit -m "un bref message qui explque les changements apportees"
```
et enfin, quand vous êtes prêts :
```console
git push  
```
A noter que la première fois le code va vous demander de vous identifier avec git --global user.name etc. Faites comme le système vous le demande (vous pouvez entrer n'importe quoi comme utilisateur@systeme.loc pour l'email et le nom d'utilisateur, tant que c'est assez unique pour que vous vous reconaissiez après)

Maintenant le code est sauvegarder sur le serveur. Vous pouvez faire le même processus (clone, add, commit, push) depuis un autre ordinateur du réseau, ce qui permet de la collaboration sur un projet. 
À noter que sur le serveur, dans le dossier votreprojet.git vous **ne verrez PAS** votre code. Il est syncroniser à chaque push mais il n'existe pas en tant que tel dans le dossier sur le serveur. C'est pourquoi pour travailler sur votre code vous devez le git clone, add, commit, push comme mentionner sur une autre machine.

![Git add, commit, push](/assets/img/Git-add-commit-push.png)  

Et voila! Maintenant passons à VSCode.

## VSCode 
![Visual Studio Code](https://img.shields.io/badge/Visual%20Studio%20Code-0078d7.svg?style=for-the-badge&logo=visual-studio-code&logoColor=white)
, ou VSCode, est, comme le dit Wikipedia :
>"Un éditeur de code extensible développé par Microsoft pour Windows, Linux et macOS.
>
>Les fonctionnalités incluent la prise en charge du débogage, la mise en évidence de la syntaxe, la complétion intelligente du code (IntelliSense.), les snippets, la refactorisation du code et **Git** >intégré. Les utilisateurs peuvent modifier le thème, les raccourcis clavier, les préférences et installer des extensions qui ajoutent des fonctionnalités supplémentaires. "
>{..}
>"Dans le sondage auprès des développeurs réalisé par Stack Overflow en 2023, Visual Studio Code a été classé comme l'outil d'environnement de développement (IDE) le plus populaire, avec plus de 73 % des 86 544 répondants déclarant l'utiliser"

C'est donc un éditeur de texte avec des fonctions intéressantes, comme le support de Git, et il est utiliser par beaucoups de monde. Dans cette section je vais vous expliquer comment syncroniser vscode avec un serveur git sur le même réseau

{: .box-note}
Note: Pour utiliser dans VSCode votre machine doit ABSOLUMENT avoir sa clée de connection à distance ssh authorisée sur le serveur. Demandez à vos administrateurs si c'est le cas.


Premièrement, cliquer sur le boutton avec 3 points reliés par des branches (comme illuster ci-dessous) :  
![Boutton source-control dans vscode](/assets/img/vscode-sourcecontrol-button.png)  

Cela ouvrira un dialogue en haut de la page. Taper la même chose quand command line (ex. git@192.168.0.132:/usr/local/git/test.git). Cela ouvrira votre explorateur de fichiers. Choisisez un dossier de destination.

VSCode vous demanderas si vous voulez ouvrir le repo qui a été cloner, appuyer sur 'ouvrir' (ou open) :  
![ouvrir rep ocloner](/assets/img/vscode-open-cloned-repo.png)  
Finnalement, cliquer sur 'oui, faire confiance' quand cela vous sera demander.  
Voila! Maintenant vous devriez voir les fichiers de votre repo (si vous l'avez déja modifier puis push au paravant)

Au fur et à mesure que vous modifier du code, rajouter des fichiers, etc, vous verrez une pastille avec des nombres sur le symbole 'source control' dans la barre laterale.
Cela represente les changement apporter localement qui n'ont pas (encore) été synchroniser avec le serveur:  
![Pastilles montrant les changement non-syncroniser](/assets/img/VScode-pastille-sourcecontrol.png)  
Nous allons donc proceder a la synchronisation.  
Cliquer sur le boutton du source control. Ensuite cliquez sur 'Commit changes'. Un dialogue apparaitra vous disant qu'il n'y a aucun changement _stager_, c'est a dire en attente d'etre envoyer en commit (en gros vscode n'a pas encore fait git add --all).   
Cliquer sur "always" (toujours). 
Cela vous ouvrira sur un nouveau fichier, probablement .git/COMMIT_EDITMSG. 
En bas des lignes commencent par '#', écriver votre message de commit. Puis, sauvegarder le fichiers (CTRL+S). 
Une autre popup apparaitra :  
![Popup pour l'edition de COMMIT_EDITMSG](/assets/img/VScode-editer-commit_editmsg.png)  
Cliquer sur Save (sauvegarder)  
Cela vous rammenera à l'écran du source-control. Cliquez sur le boutton bleu avec les flèches dans la barre laterale :  
![Vscode boutton push](/assets/img/VSCode-boutton-push.png)  
Finnalement, vous pouvez cliquer sur 'OK'.  

À noter qu'un message pourrait apparaitre dans le coin inferieur droit demandant si vous voulez que VSCode execute périodiquement la commande 'git fetch'. Cette commande permet de mettere a jour le repo cloner actuel en cas d'éventuelle mise-à-jour depuis une autre machine (si quelqu'un d'autre travaille sur le même project git que vous et push des changements, par example). Le choix d'accepter ou non est le votre.


