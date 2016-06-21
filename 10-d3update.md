---
layout: page
title: D3 - Transitions
subtitle: On bouge!
minutes: 20
---

> ## Objectifs du cours {.objectives}
> 
> * Utiliser un slider 
> * Mettre à jour des points avec d3.transition
> * Tout regrouper dans un dernier effort!

Dans notre code, l’année à laquelle nous regardons les données est codée « en dur » : il s’agit de l’année 2009, dernière de la série.

Maintenant nous souhaitons que l’utilisateur puisse observer l’évolution de ces données au fil du temps. 

Faisons cela avec un curseur de défilement (slider). La première chose à faire est d’ajouter ce slider ) notre interface (notre page). En HTML il s’agit tout simplement d’un élément `input` de type `range`. Nous lui attribuons  un identifiant de façon à pouvoir le sélectionner depuis notre script, une classe pour pouvoir lui affecter un style (si on le souhaite), et des valeurs minimum, maximum, et de pas qui dépendent de nos données. `value` est la propriété qui nous permet de savoir à quelle position le slider est réglé. Initialisons-le quelque part au milieu de notre série (1979).

~~~{.html}
<input type="range" name="range" class="slider" id="year_slider" value="1979" min="1950" max="2008" step="1" ><br>
~~~

Dans notre script, l’année est une variable, nous devons l’intialiser. 
Comme `value` est une chaîne de caractères et non un nombre, nous devons d’abord le convertir avec `parseInt()`.
Pour ensuite récupérer l’index (et non l’année), il suffit de retrancher la valeur de la première année de la série, 1950.

~~~{.js}
var year_idx = parseInt(document.getElementById("year_slider").value)-1950;
~~~

Mettre à jour l’année devient relativement simple. Il suffit d’ajouter un nouveau listener qui modifie la variable `year`lorsque nous touchons au slider. L’événement que l’on souhaite écouter s’appelle `input`. On exécute alors la fonction `update()` définie précédemment.

~~~{.js}
d3.select("#year_slider").on("input", function () {
	year_idx = parseInt(this.value) - 1950;
	update();
});
~~~

Jusqu’ici la fonction update ne sait que gérer des ajouts et des suppressions de données, avec `.enter` et `.exit`; que faire lorsque des données évoluent?
En plus de `d3.enter()` et `d3.exit()`, D3 offre `d3.transition` qui permet de gérer la mise à jour des données**. Mais d’abord il faut définir comment on passe d’une valeur à la suivante. Nous pouvons souhaiter faire une interpolation linéaire des valeurs sur une durée de 200ms, comme ceci: 

~~~{.js}
dot.transition().ease("linear").duration(200);
~~~

Il reste à dire vers quelle valeur on va effectuer la transition. 
Pour cela, déplaçons le bout de code qui met à jour le cercle, en le retirant de `enter()` pour le mettre dans la partie `transition()`. Et, à la place de l’index en dur pour aller chercher l’année, on va se baser sur la valeur de `year_idx`, qui a été mise à jour un peu avant dans notre listener.

~~~{.js}
dot.enter().append("circle").attr("class","dot")
        .style("fill", function(d) { return colorScale(d.region); });
dot.exit().remove();
dot.transition().ease("linear").duration(200)
				.attr("cx", function(d) { return xScale(d.income[year_idx]); }) // voici comment attr sait quelle donnée il doit considérer
				.attr("cy", function(d) { return yScale(d.lifeExpectancy[year_idx]); })
				.attr("r", function(d) { return rScale(d.population[year_idx]); });
~~~

> ## D’autres fonctions de transition sont possibles {.callout}
> * sin - applique la fonction trigonométrique sinus().
> * exp - exponentielle.
> * bounce - simule une collision avec un rebond.
> * elastic(a, p) - simulates un élastique; cette fonction peut dépasser un peu des bornes 0 et 1.
> * [pour en savoir plus](https://github.com/mbostock/d3/wiki/Transitions#d3_ease)

> # C’est l’heure de s’amuser {.challenge}
> D3 est incroyablement versatile. ESsayez différentes transitions et, si vous avez du temps, voyez comment dessiner des rectangles à la place des cercles.

Ensuite, nous voulons créer un pop-up. Regardons un peu ce qui existe déjà sur internet. 
Il existe beaucoup d’exemples de code utilisant D3. Voici un exemple de [simple pop-up](http://bl.ocks.org/biovisualize/1016860).
Pour suivre cet exemple nous commençons par créer la variable tooltip:

~~~{.js}
var tooltip = d3.select("body")
	.append("div")
	.style("position", "absolute")  
	.style("visibility", "hidden");
~~~

puis nous créons un listener pour les événements de survol des cercles avec la souris. Le code cidessous permet d’afficher le pays que l’on survole, le pop-up va suivre le mouvement de la souris, et quand on quitte le cercle, le popup doit disparaître.

~~~{.js}
dot.enter().append("circle").attr("class","dot")				      	
			.style("fill", function(d) { return colorScale(d.region); })
			.on("mouseover", function(d){return tooltip.style("visibility", "visible").text(d.name);})
			.on("mousemove", function(){return tooltip.style("top", (d3.event.pageY-10)+"px").style("left",(d3.event.pageX+10)+"px");})
			.on("mouseout", function(){return tooltip.style("visibility", "hidden");});
~~~

> ## Nous avons exploité différents objets donnés par le navigateur  {.callout}
> * document.x - sélectionner des choses sur la page (getElementById)
> * console.x - afficher un élément dans la console (log)
> * event.x - dans le cadre d’un événement comme “mouseover”, "mousemove", ou “keydown”. Renvoie des détails sur l’événement (pageX - où cet événement s’est-il produit dans la page?).

Comme tout langage de programmation, Javascript peut aussi servir à faire des calculs statistiques avec nos données. Comme exemple, calculons la moyenne de l’espérance de vie et du revenu par habitant dans les différentes régions. 

On commence par faire le tour des pays en les groupant par région:

~~~{.js}
// Calculate the averages for each region.
var region_names = ["Sub-Saharan Africa", "South Asia", "Middle East & North Africa", "America", "East Asia & Pacific", "Europe & Central Asia"];

var region_data = [];
for (var i in region_names) {
	var filtered_nations_by_regions = nations.filter(function(nation){
		return (nation.region == region_names[i]); 
	});
	region_data[i] = calc_mean(filtered_nations_by_regions);
}

var filtered_reg_nations = region_data.map(function(region) { return region;});
~~~

Ensuite, on crée une fonction qui renvoie un tableau d’objets region_data. On souhaite qu’il contienne la moyenne du revenu par habitant et de l’espérance de vie année par année, à travers toutes les nations, pondérées par la population.

~~~{.js}
function calc_mean(region_data) {
	var mean_income = [];
	var mean_lifeExpectancy = [];

	for (var year_idx2 in region_data[0].years) {
		var sum_income = 0;
		var sum_lifeExpectancy = 0;
		var sum_population = 0;

		for (var k in region_data) {
			var kpop = region_data[k].population[year_idx2];
			var kincome = region_data[k].income[year_idx2];
			var klife = region_data[k].lifeExpectancy[year_idx2];
		    sum_income += kpop*kincome; 
		    sum_lifeExpectancy += kpop*klife;
		    sum_population += kpop;			    
		}

		mean_income[year_idx2] = sum_income/sum_population;
		mean_lifeExpectancy[year_idx2] = sum_lifeExpectancy/sum_population;
	}
	averageData = {
		region: region_data[0].region,
		years: region_data[0].years,
		mean_income: mean_income,
		mean_lifeExpectancy: mean_lifeExpectancy
	};

	return averageData;
}
~~~

> # Le grand défi {.challenge}
> Il est temps de mettre ensemble tout ce que nous avons appris. Ecrire du code qui affiche (et met à jour) les valeurs moyennes que nous venons de calculer sous forme de petites croix dans le graphique, une croix figurant chaque région.

> # ...style! {.challenge}
> Ajouter des étiquettes aux axes et choisir de jolies polices de caractères. 

> # Utiliser d’autres formats de données {.challenge}
> Essayer de charger nations.csv à la place de nations.json - pour produire le même graphique. 

À la fin de cette leçon, votre page doit resembler à ceci:

<iframe src="http://isakiko.github.io/D3-visualising-data/code/index10.html" width="1000" height="600"></iframe>
