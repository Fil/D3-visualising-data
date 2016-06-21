---
layout: page
title: D3 - Ajouter et supprimer
subtitle: Ajout dynamique de points de données
minutes: 20
---

> ## Objectifs de la leçon {.objectives}
> 
> * Filtrer les données
> * Créer des cases à cocher
> * Ajouter et supprimer des points de données (d3.enter et d3.exit)

Notre graphique est assez chargé. On ne souhaite pas forcément tout afficher tout le temps.
L’objectif de cette leçon est de mettre à jour le graphique en fonction des données que l’on veut afficher. 

TOu d’abord il nous faut une manière de filtrer nos données. Pour cela nous allons utiliser la fonction `filter`. 
Comme la fonction `.map()` vue précédemment, cette fonction s’applique à chacun des éléments de la liste `nations` (l’appelant temporairement `nation`). 
Elle n’inclut ensuite dans la nouvelle liste `filtered_nations` que les éléments pour lesquels la fonction a renvoyé une valeur équivalente à ‘true’. Ici ce sera le cas pour les nations dont la population en 2009 était supérieure à 10 millions d’habitants.

~~~{.js}
var filtered_nations = nations.filter(function(nation){ 
	return nation.population[nation.population.length-1] > 10000000;
});
~~~

> # Filtrer par région {.challenge}
> Vous avez peut-être remarqué que nos données contiennent des informations sur la région à laquelle appartient un pays.
> 
> 1. Créer un filtre qui permette de n’afficher que des poins de données appartenant à la région d’Afrique sub-saharienne.

Nous venons de coder en dur un critère pour les données que nous voulons afficher. Naturellement, nous voulons que ce changement se fasse de manière interactive. Créons quelques cases à cocher qui nous permettront d’ajouter les régions que nous souhaitons prendre en compte.

Pour faire ceci, il nous faut revenir un moment à notre fichier HTML. Nous allons y créer une case à cocher pour chaque région. Au départ, chaque case sera cochée, pour que le graphique démarre avec tout le contenu. En HTML, une case à cocher se fait avec un élément input de type checkbox: 

~~~{.html}
<label><input type="checkbox" name="region" class="region_cb" value="Sub-Saharan Africa" checked="checked"/> Sub-Saharan Africa</label>
~~~

L’étape suivante est d’ajouter un listener qui sera activé à chaque modification des cases à cocher de la page. D3 fournit des outils très pratiques dans ce cas de figure, et permet notamment de récupérer la valeur de chaque option, qui viendra nourrir notre filtre. 

~~~{.js}
d3.selectAll(".region_cb").on("change", function () { <--- ici se passe l'action ---> });
~~~

Cette ligne écoute tous les changements d’état des cases à cocher portant la classe `region_cb`. À chaque modification d’état, qu’il s’agisse de cocher ou de décocher une case, la fonction de rappel est exécutée.  

À l’intérieur de cette fonction, on va lire les données de la case à cocher, que l’on a préalablement réglées comme étant le nom de chaque région. Le mot-clé `this` dans la fonction de rappel fait référence à l’élément qui vient d’être cliqué, on peut donc récupérer sa valeur. 

~~~{.js}
var type = this.value;
~~~

Maintenant que le nom de la région est enregistré dans la variable `type`, il nous faut ajouter les points de données correspondants si la case à cocher est activée. On vérifie si elle est cochée ou non en regardant son attribut `this.checked`. Pour ajouter les nouvelles nations à `filtered_nations`, on utilise la fonction `concat` qui met bout à bout deux tableaux. On ajoute donc `new_nations` à la fin de `filtered_nations`. 

~~~{.js}
if (this.checked) { // adding data points 
  var new_nations = nations.filter(function(nation){ return nation.region == type;});
  filtered_nations = filtered_nations.concat(new_nations);
}
~~~

Cette clause `if` est exécutée à chaque fois qu’on clique sur une case à cocher. Pour ajouter les points de données, on peut utiliser la fonction `push`, qui ajoute les éléments un par un. 
Pour cela, on filtre les nations qu’on veut ajouter, en les appelant `new_nations`. Ensuite on boucle sur toutes ces nations et on les ajoute une par une à la liste `filtered_nations`.

Au préalable il aura fallu initialiser `filtered_nations` au début de notre script. Comme il existe une différence entre l’espace des noms et l’espace des objets, il faut se souvenir d’utiliser `.map()` pour cloner les données, et pas seulement `=`.

~~~{.js}
var filtered_nations = nations.map(function(nation) { return nation; });
~~~

Au départ `filtered_nations` est identique à `nations` car toutes les cases sont cochées, on affiche les données de toues les nations. Cela signifie que tout changement dans les cases à cocher à cette étape les fera basculer dans l’état « non coché »; il nous faut donc un code à actvier pour enlever des éléments quand une case est décochée. 

Avant de faire ceci, nous devons apprendre à supprimer des éléments en D3. Cela se fait avec la fonction `exit()`. 

~~~{.js}
dot.exit().remove();
~~~

Là où on utilisait `enter()` pour ajouter de nouveaux éléments au grpahique, `exit()` va nous permettre de supprimer des éléments qui ne sont plus présents dans le jeu de données (les pays appartenant à des régions décochées). Les deux fonctions comparent les données demandées à celles qui sont sur le graphique. Comme pour `enter()`, tout ce qui suit `exit()` est actionné pour chacun des éléments dont les données ont « disparu ». Ici, comme dans la plupart des cas, on va vouloir supprimer ces éléments graphiques. 

Une bonne explication de ce lien entre les données et les éléments est formulée [ici](http://bost.ocks.org/mike/join/). Cet article détaille les trois fonctionnalités importantes: `enter`, `exit`, et une troisième, `update`, que nous allons bientôt présenter. 

> # Supprimer des éléments {.challenge}
> 1. En utilisant un cas `else` après le `if`, créer un filtre qui supprime les éléments de `filtered_data` qui correspondent aux cases à cocher qui viennent d’être décochées. (par exemple `else { filtered_nations = <--- remplir cette partie --->}`). 


Dernière étape : déplaçons `enter()` et `exit()` dans une fonction à part. Ceci sera utile quand nous voudrons mettre à jour les données de différents éléments de la page. 

~~~{.js}
function update() {
  var dot = data_canvas.selectAll(".dot").data(filtered_nations, function(d){return d.name});

  dot.enter().append("circle").attr("class","dot")                
                .style("fill", function(d) { return colorScale(d.region); });
                .attr("cx", function(d) { return xScale(d.income[d.income.length-1]); }) // this is how attr knows to work with the data
                .attr("cy", function(d) { return yScale(d.lifeExpectancy[d.lifeExpectancy.length-1]); })
                .attr("r", function(d) { return rScale(d.population[d.population.length-1]); });

  dot.exit().remove();
}
~~~

Désormais, nous pouvons appeler cette fonction de mise à jour (update) depuis notre listener qui surveille les changements d’état des cases à cocher, juste après avoir mis à jour la sélection `filtered_nations`:

~~~{.js}
d3.selectAll(".region_cb").on("change", function() {
  var type = this.value;
  if (this.checked) { // adding data points (not quite right yet)
    var new_nations = nations.filter(function(nation){ return nation.region == type;});
    filtered_nations = filtered_nations.concat(new_nations);
  } else { // remove data points from the data that match the filter
    filtered_nations = filtered_nations.filter(function(nation){ return nation.region != type;});
  }
  update();
});
~~~

Pour créer le graphique à l’ouverture de la page, on appelle aussi `update()` une fois hors des listeners.

> # Ajouter une dimension {.challenge}
> 1. On souhaite que la couleur des cercles représente la région. Créer une échelle de couleurs avec category20(). Il faudra alors ajouter `.style("fill", function(d) { <-- remplir cette partie ---> });` dans la fonction enter().

Au terme de cette leçon, votre page devrait ressembler à ceci:

<iframe src="http://isakiko.github.io/D3-visualising-data/code/index09.html" width="1000" height="600"></iframe>
