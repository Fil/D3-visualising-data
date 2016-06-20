---
layout: page
title: HTML
subtitle: Faire apparaître des choses dans votre navigateur
minutes: 20
---

> ## Objectifs de la leçon {.objectives}
>
> * Créer votre première page web et la visualiser dans un navigateur
> * Comprendre la structure d’un document HTML
> * Distinguer les divers composants d’un document HTML

Tous les navigateurs Web comprennent les documents écrits en langage HTML: hypertext markup language. Mais ce langage ne permettant pas de procéder à des opérations logiques (boucles, etc.), on ne peut techniquement l’appeler langage de programmation.

Voyons tout d’abord comment on peut amener notre navigateur à saluer. 
Nous allons:

* Créer un répertoire local 'my_first_webpage'
* Dans ce répertoire, créer un fichier nommé 'index.html'
* Ouvrir ce fichier avec un éditeur de texte

Ensuite, pour ouvrir ce fichier dans un navigateur, il faut commencer par lui dire de quel type de fichier il s’agit. Pour cela, nous inscrivons comme premières lignes dans notre fichier:

~~~ {.html}
<!DOCTYPE html>
<html>
	--> Tout le reste viendra ici <--
</html> 
~~~

Une page web bien formée comprend un entête (head) et un corps (body). 
L’entête (&lt;head&gt; pour l’ouvrir et &lt;/head&gt; pour le fermer) devra normalement comporter les méta-données. COmme par exemple le nom de la page, ou une liste d’autres fichiers à inclure. 

Le corps (entre &lt;body&gt; et &lt;/body&gt;) est là où nous allons placer notre contenu. Tout ce que nous entrons entre ces balises sera affiché sur notre page.

~~~ {.html}
<head> 
</head>
<body> 
	Salut!
</body> 
~~~

Comme notre navigateur sait interpréter le langage HTML, nous pouvons instantanément ouvrir le fichier local index.html file et le navigateur transformera le code en éléments visuels. 

HTML comprend beaucoup d’éléments pré-définis, qui permettent de varier les styles et tailles d’affichage. 
Pour séparer la page en diverses sections, nous pouvons utiliser la balise &lt;div&gt; (pour ouvrir) et &lt;/div&gt; pour fermer chaque division. 

~~~ {.html}
<!DOCTYPE html>
<html> 
	<head> 
		--> meta-data (titre de la page, inclusion d'autres fichiers) <--
	</head> 
	<body> 
		<div>Salut le monde!</div>
		<div>Salut à toi!</div>
	</body> 
</html> 
~~~

> ## Autres éléments {.challenge}
>
> Créer un répertoire qui contient le fichier index.html.
> Quelle est la différence entre &lt;div&gt;, &lt;h1&gt;, et &lt;em&gt;?
> Quelle est la différence entre deux éléments &lt;div&gt; successifs, et deux éléments &lt;span&gt; successifs?
> Créer un intertitre en italiques. Ne craignez pas d’utiliser un moteur de recherche pour retrouver la syntaxe!

On trouvera des ressources utiles sur le Mozilla Developer Network ([MDN](https://developer.mozilla.org/fr/)) et sur le site [pompage](https://www.pompage.net/). 
