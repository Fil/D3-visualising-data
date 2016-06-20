---
layout: page
title: Publier et partager avec Github
subtitle: Montrez vos réalisations! 
minutes: 20
---

> ## Objectifs de la leçon {.objectives}
>
> * Héberger votre page sur Github

Notre prochain objectif est de partager ce que nous venons de réaliser. 
Une des manières les plus simples de réaliser cela est fournie par [Github](https://github.com). 

Il faudrait que chacun dispose d’un compte sur Github. SI vous n’en avez pas encore, c’est le moment de le créer.

Github utilise git, qui est un logiciel de suivi de versions, c’est-à-dire qu’il permet de suivre les modifications de contenus dans des fichiers et d’en documenter l’historique. Cela le rend parfait pour héberger du code, car cela nous permettra de revenir en arrière autant que nous le voudrons, et d’expérimenter sans crainte de perdre notre travail, avec la possibilité à tout moment d’annuler d’éventuelles erreurs. 

En utilisant git on crée ce qu’on appelle des « repositories » dans nos dossiers. Le dossier concerné et ses sous-dossiers sont alors suivis et git indique immédiatement ce qui a été modifié dans les fichiers de ce répertoire.

On peut alors « commiter » une modification, et synchroniser ensuite cette modification avec l’historique conservé chez Github. 

Mieux encore, Github ouvre la possibilité de transformer n’importe quel « repository » en site web. 

Mais créons d’abord notre premier repository sur Github:

* Connectez-vous à Github (https://github.com)
* Créez un nouveau repository et appelez-le 'myartwork'

Ne vous préoccupez pas de comprendre toutes les possibilités, dans un premier temps. On peut utiliser git en ligne de commande, et il existe également une application Github pour Windows ou Mac OS X, ou encore Sourcetree (https://www.sourcetreeapp.com/). Avec l’un de ces logiciels, on peut cloner son repository pour en faire une copie locale. 

Recopiez ensuite le fichier qui contient votre œuvre d’art dans le répertoire local, et appelez-le 'index.html'.
(‘index.html’ est le nom par défaut du fichier qui sert de page d’accueil sur un serveur web).

Enregistrez le fichier, commitez la modification, et « poussez » la synchronisation vers le repository sur Github.

Désormais, notre code est sur le web. Mais nous n’avons pas encore expliqué à Github que nous souhaitions le transformer en site. 
Pour cela, nous créons une branche que nous appelons 'gh-pages'. Sur le site de Github, naviguez jusqu’au repository et cliquez sur l’expression "branch:master". On vous invite alors à saisir le nom d’une autre branche; entrez 'gh-pages' et validez. Une nouvelle branche est alors créée, et votre page doit apparaître à l’adresse: http://utilisateur.github.io/myartwork . Dès lors, toute synchronisation avec la branche ‘gh-pages’ sera validée sur le site.

> ## Publiez votre réalisation! {.challenge}
>
> Publiez votre réalisation et envoyez-nous un lien, sous la forme http://username.github.io/myartwork
