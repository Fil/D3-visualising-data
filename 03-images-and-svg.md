---
layout: page
title: Images et SVG
subtitle: Ajouter des éléments graphiques
minutes: 20
---

> ## Objectifs de la leçon {.objectives}
>
> * Ajouter des images dans la page
> * Comprendre le format Scalable Vector Graphics (SVG) 
> * Ajouter plusieurs SVG dans la page

HTML permet d’ajouter une image très simplement dans les pages. 

~~~{.html}
<img class='image' src="cat.jpg">
~~~

On remarque que cet élément IMG ne demande pas de balise fermante : l’idée est qu’il n’y a pas de contenu « à l’intérieur » de l’image.

Pour la suite, il va falloir télécharger une image de chat ; vous pouvez en trouver une dans notre dépôt.

An ajoutant une classe 'image' à notre fichier CSS, nous pouvons définir la taille et la position de l’image que nous venons de charger, et appliquer le même style à d’autres images par la suite. 

~~~{.css}
.image {
	width: 200px;
	position: relative;
	left: 20px;
}
~~~

Notre objectif final est de créer un graphique, qui sera composé d’éléments tels que des lignes, des cercles, ou des rectangles (et pas des photos de chats). 
Bien sûr nous pourrions chercher l’image d’un cercle et l’utiliser pour représenter nos données en le retaillant et en le positionnant sur la page, mais ce n’est pas la méthode la plus adaptée. 

Une meilleure approche pour traiter d’élements graphiques qui ne sont pas des photos, est d’utiliser le format SVG (Scalable Vector Graphics : graphiques vectoriels redimensionnables).

Un SVG peut être utilisé comme un format de fichier à part, mais on peut aussi l’écrire directement à l’intérieur du HTML, tout comme les éléments ‘div’ ou ‘h1’ vus précédemment.

~~~{.html}
<svg class="chart">
 	<circle cx="25" cy="25" r="15" class="circ1">
 	</circle>
</svg>
~~~

Ici, nous venons de créer un fond SVG, avec les styles de la class ‘chart’.
Sur ce fond, nous avons tracé un cercle, avec les styles de la classe 'circ1'.
Ces deux classes doivent être définies dans notre fichier CSS:

~~~{.css}
.chart {
	width: 100px;
	height: 100px;
}

.circ1 {
	stroke: green; 
	fill: white;
	stroke-width: 5;
}
~~~

L’élément circle est déjà défini. ‘cx', 'cy', et 'r' sont des attributs spécifiques à cet élément. ‘cx' et 'cy' definissent les coordonnées x et y du centre du cercle, et  'r' est son rayon. 

Au-delà du cercle, d’autres formes sont possibles, dont on trouvera facilement des exemples sur Internet. Une excellente ressource pour des exemples avec SVG est [w3schools](http://www.w3schools.com/svg/default.asp).

> ## Question {.challenge}
>
> Que se passe-t-il si 'cx' et 'cy' n’ont pas été précisés?

> ## Faire de l’art! {.challenge}
>
> Dessiner quelque chose, à partir de formes comme un cercle, un rectangle, un polygone.
> Par exemple, dessiner un robot! 
