---
layout: page
title: Mise en place de D3
subtitle: Un peu de html pour notre visualisation de données
minutes: 20
---

> ## Objectifs de la leçon {.objectives}
> 
> * Créer un fichier HTML qui contiendra le graphique
> * Lire des données à partir d’un fichier `.json`
> * Structurer le contenu HTML

Avec tout ce que nous avons vu précédemment, nous pouvons désormais commencer à utiliser D3. 
D3 est une bibliothèque Javascript. Il s’agit d’un ensemble de fonctions que l’on rajoute aux commandes Javascript que l’on connaît déjà, et qui nous facilitent la vie.

Le but principal de D3 est de créer des visualisations en ligne. Comme il est écrit en Javascript, ces visualisations pourront facilement être interactives! 

Mais commençons par mettre en place notre page HTML. 

Créez un nouveau repository sur GitHub, et une branche gh-pages branch dans laquelle nous allons commiter. C’est là que se trouvera notre page.
Puis créez `index.html` dans le nouveau repository, avec le contenu suivant:

~~~{.html}
<!DOCTYPE html>
<html>
  <head>
    <title>Richesse & Santé des Nations</title>
    <link rel="stylesheet" type="text/css" href="main.css" />
  </head>
  <body>

    <h1>Richesse & Santé des Nations</h1>

    <p id="chart_area"></p>

    <script src="http://d3js.org/d3.v3.min.js"></script>
    <script src="main.js"></script>
  </body>
</html>
~~~

Il y a plusieurs nouveautés dans ce fichier:
`<link rel="stylesheet" type="text/css" href="main.css" />` crée un lien vers le fichier css local `main.css`(qu’on peut laisser vide pour le moment). `<script src="main.js"></script>` appelle le fichier Javascript dans lequel l’action va se dérouler. 

De plus, nous créons un lien vers la bibliothèque d3 en appelant `<script src="http://d3js.org/d3.v3.min.js"></script>`. L’ordre est important. Notre script faisant appel aux fonctions définies dans D3, le lien vers d3.js doit apparaître avant l’appel de notre propre script.

Dernier point, il est important de créer un élément HTML (un paragraphe). Nous lui donnons l’identifiant `chart_area`. C’est l’emplacement que l’on réserve pour notre joli graphique. Nous allons utiliser Javascript (et D3) pour le remplir. 


Maintenant, il s’agit d’écrire le script main.js.

La syntaxe des fonctions de D3 utilise `d3.xxx`, tout comme d’autres fonctions Javascript (par exemple `JSON.stringify`).

La première chose dont nous avons besoin, c’est nos données, qui se trouvent à l’adresse 'https://raw.githubusercontent.com/IsaKiko/D3-visualising-data/gh-pages/code/nations.json'.
D3 offre une function très pratique pour lire des fichiers au format `json`:

~~~{.d3}
var dataUrl = "https://raw.githubusercontent.com/IsaKiko/D3-visualising-data/gh-pages/code/nations.json";
d3.json(dataUrl, function(nations) { })
~~~

Cette ligne demande quelques éclaircissements, et nous allons la parcourir pas à pas: 

* `d3.json()` est l’appel de la fonction. Dans ce cas, nous avons une fonction qui lit un fichier JSON, l’analyse, et envoie les résultats à une fonction callback.
* Son premier argument `dataUrl` indique où se trouvent les données que nous voulons lire.
* `function(...){...}` est une fonction de rappel (callback). Elle sera exécutée une fois que les données auront été chargées et analysées.
* D3 crée une variable nommée `nations`, qui contiendra l’objet analysé par la fonction. Cette variable 'nations' n’est disponible qu’à l’intérieur de la fonction de rappel; autrement dit le code ne connaît ces valeurs qu’à l’intérieur de ces accolades.
* Il faut noter que cette fonction ne renvoie rien. Elle est exécutée et lance ensuite, de manière asynchrone, une fonction de rappel qui s’exécutera à son tour quand les données auront été chargées, et pourra faire des affichages. 


> ## Quels autres formats de données sont accesibles facilement? {.callout}
> D3 offre la possibilité de lire directement des fichiers CSV (valeurs séparées par des virgules). Voir [cette page](https://github.com/mbostock/d3/wiki/CSV) pour un exemple. D’autres fonctions permettent de lire des fichiers de données séparées par des tabulations (tsv) ou un autre délimiteur arbitraire (dsv).



La question désormais est de réfléchir à ce que nous voulons effectuer à l’intérieur de ces accolades, là où nous venons de récupérer des données.
Dans un premier temps, nous allons essayer de:

* Associer le Javascript à la page HTML
* Créer un fonds SVG
* Créer des axes (x: revenu par habitant, y: espérance de vie)
* Afficher des points représentant les données (scatter plot/nuage de points)

Commençons par un petit schéma de la page que nous voulons obtenir.

<img src="img/chart_area.png" alt="Ce que nous voulons obtenir..." width="700" />

Notre page HTML contient déjà l’emplacement où l’on va ajouter notre zone de graphique. 
Nous allons y créer un fonds SVG (un élément SVG), une zone de tracé (un élément g), et, dans cette zone, deux éléments distincts, deux « calques », qui recevront respectivement les axes pour l’un, et les cercles pour l’autre.

Dans un premier temps, nous voulons donc obtenir ceci:

~~~{.html}
<p id="chart_area"> <svg> </svg> </p>
~~~

Mais cette fois, nous allons créer ces éléments directement en Javascript.
Pour relier le Javascript et l’environnement HTML, nous utilisons la fonction `d3.select()`. Ceci nous permet d’attraper un élément, en précisant son identifiant.

~~~{.js} 
// Sélectionner la zone de graphique avec son ID 
var chart_area = d3.select("#chart_area");
~~~

Maintenant on ajoute le cadre SVG.

~~~{.js} 
var frame = chart_area.append("svg");
~~~

Le résultat est mémorisé dans la variable `frame`, ce qui fait qu’on pourra le référencer sans avoir à le sélectionner par son identifiant.

On crée alors dans cet élément SVG le groupe `g` qui contiendra le graphique:

~~~{.js}
// Créer le fonds dans le cadre.
var canvas = frame.append("g");
~~~

Fixons maintenant les dimensions de nos éléments:

~~~{.js}
// Régler les marges, largeur, hauteur.
var margin = {top: 19.5, right: 19.5, bottom: 19.5, left: 39.5};
var frame_width = 960;
var frame_height = 350;
var canvas_width = frame_width - margin.left - margin.right;
var canvas_height = frame_height - margin.top - margin.bottom;
~~~

…et les appliquer aux éléments définis:

~~~{.js}
// Fixer les attributs  width et height du cadre.
frame.attr("width", frame_width);
frame.attr("height", frame_height);
~~~

Pour que le fonds (l’élément g) s’intègre bien dans le cadre, on le décale un petit peu, avec une marge, que l’on va préciser avec un attribut `transform: translate(…)`. 

~~~{.js}
// Décaler le fonds à l'intérieur du cadre.
canvas.attr("transform", "translate(" + margin.left + "," + margin.top + ")");
// Fixer sa taille
canvas.attr("width", canvas_width);
canvas.attr("height", canvas_height);
~~~

> # Ajouter des éléments SVGs depuis le fichier Javascript {.challenge}
> 1. Ajouter un élément `circle` SVG au cadre.
> 1. Régler le rayon du cercle à 40px, son contour en noir et sa couleur de fond en vert.
>
> Indice: Vous pouvez utiliser la méthode `.attr` sur l’objet circle obtenu.

