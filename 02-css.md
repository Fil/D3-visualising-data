---
layout: page
title: CSS
subtitle: Le faire avec style
minutes: 20
---

> ## Objectifs de la leçon {.objectives}
>
> * Modifier l’apparence de divers éléments HTML
> * Créer des classes dans un fichier CSS
> * Appliquer ces classes à divers éléments HTML 

Le titre a une certaine apparence. Ce look (ce style) comprend la couleur, la position, la taille des caractères, et d’autres attributs.. 

Nous pouvons modifier l’apparence de notre texte de diverses manières. 
Une façon rapide est de simplement indiquer ces commandes dans son attribut "style".
Par exemple, pour modifier la couleur, on écrit:

~~~ {.html}
<h1 style="color:blue">Voici un titre bleu</h1>
~~~

Pour modifier la taille de la police: 

~~~ {.html}
<h1 style="font-size: 80px">Un GROS titre</h1>
~~~

Pour modifier deux styles, il suffit de les additionner en les séparant par un point-virgule:

~~~ {.html}
<h1 style="font-size: 80px; color: blue">Un titre gros et bleu</h1>
~~~

Cette méthode est simple et rapide et permet de modifier les élements sur place.
Cependant, si nous voulons modifier plusieurs éléments de la même manière, il va falloir saisir les mêmes informations à chaque fois, et le fichier va devenir confus et difficile à gérer sur le long terme. Mieux vaut tenter de garder séparés le fond et la forme. 

À la place de ces style= codés « en dur », il vaut mieux créer un fichier de styles à part (avec l’extension .css).

* Créer un fichier CSS: styles.css

Dans ce fichier nous allons définir des classes, que nous appliquerons ensuite à certains élements de notre fichier HTML. 
Créons par exemple une classe nommée 'title'.

~~~ {.css}
.title
{
	color: red;
	font-size: 50px;
	text-align: center;
}
~~~

Il ne reste alors plus qu’à expliquer dans notre fichier HTML où trouver la feuille de styles CSS. Pour cela on crée ce lien dans la partie head du fichier: 

~~~ {.html}
<!DOCTYPE html>
<html> 
	<head> 
		<link rel="stylesheet" type="text/css" href="styles.css">
	</head> 
~~~

Dans le corps du fichier, on peut dès lors utiliser la classe que l’on vient de créer:

~~~ {.html}
	<body> 
		<div class="title"> Premier titre </div>
		<div> du texte </div>
		<div class="title"> Autre titre </div>
	</body> 
</html> 
~~~

> ## Créer et utiliser sa propre classe {.challenge}
>
> Créez une classe nommée 'description' et un élément 'div' contenant du texte, et portant cette classe.
> Rendre le texte en gris foncé avec une taille de caractères personnalisée. 
> Ajouter un contour noir sur la gauche du ‘div’ et ajouter une marge intérieure (padding) autour du texte. 
> Ajouter suffisamment de texte pour former plusieurs lignes, et modifier la largeur de la div pour voir ce que cela donne. 
> Modifier la couleur de fond de l’élément ‘div’. 
> Si vous le souhaitez, jouez aussi avec la classe ‘title’ jusqu’à obtenir un résultat qui vous plaise. 

Afin de voir comment les éléments sont stylés, on va ouvrir les outils pour développeurs (l’« inspecteur » présent dans tous les navigateurs modernes). Pour activer cet outil, un clic-droit n’importe où sur la page offre un menu qui permet d’« inspecter l’élément ». À partir de là, on peut analyser les différentes propriétés CSS de notre page. On peut en profiter pour explorer les différents onglets de l’inspecteur.