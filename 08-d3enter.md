---
layout: page
title: D3 - Faites entrer les données
subtitle: Enfin du dessin !
minutes: 20
---

> ## Objectifs de la leçon {.objectives}
> 
> * Créer les axes
> * Afficher des données (d3.enter)


On veut construire nos axes à partir des données. Cela signifie qu’il faut d’abord connaître les valeurs minimum et maximum de nos données, et décider quel type d’axe nous voulons (linéaire ou logarithmique, par exemple).


~~~{.js}
// Créer une échelle logarithmique pour le revenu
var xScale = d3.scale.log(); // revenu
xScale.domain([250, 1e5]); // valeurs minimum et maximum
xScale.range([0, canvas_width]); // emplacement minimum et maximum sur la page
~~~

L’objet scale de D3 fournit un grand nombre de fonctions permettant de créer des échelles de façon pratique et logique. Par exemple, on peut choisir la progression de l’échelle, qu’elle soit logarithmique (`log`), linéaire (`linear`), en racine carrée (`sqrt`), ou encore catégorique (l’échelle `category20` peut représenter 20 classes de données par 20 couleurs différentes).

Le `domain` correspond aux valeurs des données qui seront converties en positions sur la page; les positions sont spécifiées par le `range`. Souvent, le domaine est réglé sur les valeurs minimum et maximum des données, et le range sur les bords de la zone de tracé du graphique.

Mais ce n’est pas obligatoirement le cas. Pensons par exemple à un zoom sur une partie des données : on changerait alors le domaine, sans toucher au range.


Le fonctionnement des objets de d3 permet d’avoir une notation plus lisible, sur une seule ligne, avec le même résultat:

~~~{.js}
var xScale = d3.scale.log().domain([300, 1e5]).range([0, canvas_width]);  
~~~

Ces deux manières d’écrire du code sont parfaitement interchangeables et c’est à vous d’utiliser celle qui vous paraît la plus intuitive. 

L’étape suivante est la création des axes, en les reliant à l’échelle que nous venons de créer:

~~~{.js}
// Creating the x & y axes.
var xAxis = d3.svg.axis().orient("bottom").scale(xScale);
~~~

`orient()` définit l’orientation de l’axe et la position des marques. Il restera à positionner l’axe sur le cadre. 

Jusqu’ici, l’axe défini ne s’affiche pas sur la page.
Pour le pousser sur notre fonds, il faut créer un élément g (avec `.append`).

~~~{.js}
// Ajouter l'axe des x.
canvas.append("g")
	.attr("class", "x axis")
  .attr("transform", "translate(0," + canvas_height + ")")
  .call(xAxis);
~~~

`.call` appelle l’axe que nous venons de créer et le pousse sur l’élément g.
On ajoute un attribut transform, qui permet de déplacer l’axe vers le bas du graphique (plutôt qu’il ne reste tout en haut à la position y=0). L’attribut transform a beaucoup d’options possibles (mise à l’échelle, rotation…), mais ici nous utilisons seulement la translation (`translate`), avec les valeurs dans les directions x et y. Ici on le déplace uniquement vers le bas, dans la direction y, sur la hauteur de notre graphique. 
On lui donne également une classe, juste pour le cas où nous voudrons par la suite le sélectionner dans notre code.

> # Nous aurons aussi besoin d’un axe des y {.challenge}
> 1. Créer une échelle linéaire pour l’axe des y, avec un minimum de 10 et un maximum de 85. Puis ajouter l’axe au fonds SVG.

On commence à se rapprocher. Avec nos deux axes, on peut maintenant ajouter nos données. 

Nous allons ajouter un cercle par point de données!
Mais on ne veut pas voir toutes les données, on va d’abord se limiter aux données les plus récentes, les chiffres de l’année 2009.

<img src="img/data_structure.png" alt="data structure" width="500" />


~~~{.js}
var data_canvas = canvas.append("g")
  .attr("class", "data_canvas");
      
var dot = data_canvas.selectAll(".dot")
  .data(nations, function(d){return d.name});

dot.enter().append("circle").attr("class","dot")
  .attr("cx", function(d) { return xScale(d.income[d.income.length-1]); }) 
  .attr("cy", function(d) { return yScale(d.lifeExpectancy[d.lifeExpectancy.length-1]); })
  .attr("r", 5);
~~~

On commence donc par ajouter un élément `g` à notre fonds.
Ce groupe sera l’emplacement où viendront se positionner les données, on lui donne une classe portant ce nom `data_canvas`.
On sélectionne alors tous les éléments de la classe `dot`. Cette sélection est vide, pour le moment.
Puis nous indiquons à notre page dans quelle variable se trouvent les données, en utilisant `.data(nations)`.

On emploie ici une fonction de clé `.data(nations, function(d){return d.name});`. Cette fonction permettra à D3 de suivre les données quand on commencera à les modifier: chaque cercle tracé correspondant à un pays, on l’associé avec cette clé. Il faut absolument que cette clé soit unique.

Et c’est ici qu’opère la magie:
La fonction `enter()` prend chaque élément du jeu de données, et applique tout ce qui suit à chacun des éléments que l’on a ajoutés. Ces nouveaux points vont être ajoutés avec la classe 'dot', ce qui permettra de les récupérer au prochain appel de `data_canvas.selectAll(".dot")`.

Maintenant nous allons créer un cercle pour chaque point de données. C’est le rôle des quatre lignes de code qui suivent. Elles créent le cercle, puis règlent ses attributs  `cx`, `cy`, et `r`. 
Les attributs `cx` et `cy` définissent la position du centre du cercle et sont basés sur le revenu (on utilise la donnée la plus récente avec le code: `[nation.income.length-1]`.) et l’espérance de vie, que l’on trouve dans notre donnée (temporairement appelée `d`). Le rayon pour l’instant est fixée à une valeur arbitraire.


> # Une dimension supplémentaire {.challenge}
> Modifier le code de façon à ce que le rayon des cercles représente la population. Pour cela il faudra créer une échelle 'sqrt' avec un minimum égal à 0 et un maximum de 5e8. Le range devrait aller de 0 à 40 pixels. 


<iframe src="http://isakiko.github.io/D3-visualising-data/code/index08.html" width="1000" height="600"></iframe>
