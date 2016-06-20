---
layout: page
title: JSON
subtitle: Tout n’est que chaînes!
minutes: 20
---

> ## Objectifs de la leçon {.objectives}
>
> * Comprendre les types de données de Javascript
> * Convertir des données Javascript au format JSON
> * Convertir des données JSON au format Javascript

Le but de ce cours est de visualiser des données. 
Nous n’avons pas encore commencé à nous occuper de données particulières, sans même parler d’importants fichiers de données.
Dans cette leçon nous allons voir les types de données de base utilisés par Javascript et évoquer divers moyens pratiques pour les convertir dans un fichier texte, afin de pouvoir les enregistrer.


Il y a deux types de conteneurs en Javascript: 
les listes ou tableaux (notation `[]`) et les objets (notation `{}`).

Pour créer un conteneur, il faut le déclarer d’abord avec le mot-clé `var`, que nous avons déjà rencontré.

~~~{.js}
var liste_de_nombres = [];
~~~ 

crée un tableau. Pour l'instant, celui-ci est vide. 

On peut maintenant ajouter des éléments dans ce tableau, ou alors le faire dès la création du tableau, sous cette forme:

~~~{.js}
var liste_de_nombres = [30, 2, 5];
~~~

Utilisons la console du navigateur pour visualiser les valeurs de cet objet:

~~~{.js}
console.log(liste_de_nombres);
~~~

On peut aussi aller chercher le _x_ème élément du tableau. Comme on compte à partir de 0, le troisième champ a l’index '2'.

~~~{.js}
console.log(liste_de_nombres[2]);
~~~

`liste_de_nombres` est une liste de 3 nombres. 

À noter, une chaîne peut être vue comme une liste de lettres:

~~~{.js}
var text = 'I love cats.';
~~~

`console.log(text[2])` renverra `l`.

Contrairement aux listes qui vous permettent d’accéder aux éléments par leur index, les objets Javascript permettent d’accéder aux propriétés (ou attributs) en utilisant des noms. 
Ce mécanisme permet de créer quelque chose du genre:

~~~{.js}
var cat_object = {
	weight : 5,
	past_weight_values : [4.5, 5.1, 4.9],
	name : 'Princess Caroline'
};
~~~

> ## Créer de nouveaux attributs {.challenge}
> On peut ajouter un attribut avec la syntaxe à point `cat_object . attributename = ...`,
> Créer un nouvel attribut `height` et affectez-lui une valeur! 

On voudra ensuite prendre une liste d’objets ; on pourra alors extraire un objet de cette liste en demandant son index.
Dans notre cas, on pourrait avoir un de multiple chats, sous forme d’une liste d’objets où chaque objet représenterait un chat. 
Pour ajouter une valeur (ou un objet) dans une liste, on utilise la fonction `push`:

~~~{.js}
var cat_list = [cat_object]; // la liste contient notre premier chat
cat_list.push({weight : 6 , past_weight_values : [5.9, 5.3, 6.1], name : 'Snowball'}); // ajout du second chat
~~~

Ce processus s’appelle 'nesting'.

> # Nesting {.challenge}
> 1. Ajouter un troisième chat dans la liste, sans préciser son nom ni son poids.
> 2. Les animaux doivent-ils tous avoir les mêmes attributs?
> 3. Utiliser la console pour lire les valeur de vos objets. 


> ## Attention aux références {.challenge}
> Que se passe-t-il si l’on référence deux fois de suite le même chat dans la liste? 
>
> Réinitialisons la liste:
>
>~~~{.js}
>var cat_list2 = [cat_object, cat_object]; 
>~~~
> 
> 1. Changeons le nom du premier chat `cat_list2[0]`. Regardons à nouveau la liste (`cat_list2`). Que s’est-il passé?
> 2. Regardons l’objet initial cat_object. Que se passe-t-il?

En Javascript, il existe un espace des noms et un espace des objets. 
L’espace des noms contient les références des variables, tandis que l’espace des objets contient le contenu réel. 
Ainsi dans notre liste, plusieurs références (le nom du chat0, le nom du chat1), pointent vers le même objet. C’est cet objet que nous avons changé, et toutes les références continuent de pointer vers cet objet.

<img src="img/namespace.png" alt=« Espace de noms et espaces des objets" width="500" />

Pour créer une nouvelle liste à partir d’une liste existante, une bonne méthode est d’utiliser la méthode `map()`, qui va appliquer une fonction à l’ensemble des éléments de la liste initiale, et renvoyer la liste des résultats. 

Voici par exemple comment produire une liste de chiens à partir de notre liste de chats. On conserve les mêmes noms, mais en leur rajoutant le mot « Doggie ».

~~~{.js}
dog_list = cat_list.map(function(cat) {
	return {
		name: "Doggie "+ cat.name
	};
});
~~~

La fonction de callback appelée par la méthode `.map()`  va s’appliquer à tous les éléments de la liste, temporairement nommés `cat`.

> # Quel est le poids des chiens? {.challenge}
> Supposns que nos chiens pèsent chacun le double de leur ancètre chat. Créez la nouvelle liste de cette manière.

Lorsque nous allons créer nos données pour le graphique, ce sont des données strucutrées de cette manière qu’il nous faudra.

Mais pour enregistrer ces données, il faut les mettre dans un format texte. C’est la fonction `JSON.stringify()` qui se charge de ce travail. 

Pour convertir notre liste de chats en un format JSON, onous allons saisir la commande:

~~~{.js}
var cat_json = JSON.stringify(cat_list);
~~~ 

La chaîne de caractères résultante est à enregistrer dans un fichier `.json`, que l'on pourra lire plus tard. 
Bien sûr à l'intérieur du navigateur il n'est pas permis de créer des fichiers, car le danger serait alors de favoriser la création de virus.

Mais on peut regarder notre chaîne de caractères avec la fonction `alert()`:

~~~{.js}
alert(cat_json);
~~~

Le résultat ressemble à ceci: 

~~~{.out}
[{"weight":5,"past_weight_values":[4.5,5.2,4.9],"name":"Princess Caroline"},{"weight":6,"past_weight_values":[5.9,5.3,6.1],"name":"Snowball"}]
~~~

On peut maintenant recopier cette chaîne et l’enresigtrer à la main dans un fichier `.json`. 

> ## Convertir en JSON  {.challenge}
> À l’inverse, les données sont parfois présentées dans des fichiers au format JSON, et on veut lire ces fichiers pour récupérer les données structurées dans le navigateur. Ce processus s’appelle « phraser » (parse), et la fonction Javascript qui s’en occupe est `JSON.parse()`
> Convertissez la variable `cat_json` en objet Javascript.

Nous sommes maintenant arrivés au point où il va falloir ouvrir le fichier de données qui nous intéresse. Ouvrons donc dans un éditeur de texte le fichier 'nations.json'.

Voici un apeçu de ce fichier. Nous avons des données de 10 pays différents, qui décrivent le revenu par habitant, la population, l’espérance de vie, pour les années 1800 à 2009.
Source: [gapminder](http://www.gapminder.org/data/)

~~~{.out}
[
  {
    "name": "Angola",
    "region": "Sub-Saharan Africa",
    "years": [
      1950,
      1951,
      1952,
	  ...
    ],
    "income": [
      3363.02,
      3440.9,
      3520.61,
	  ...
	],
    "population": [
      4117617,
      4173095,
      4232095,
	  ...
   	],
    "lifeExpectancy": [
      29.22,
      29.42,
      29.81,
	  ...
	]
  },
  { ... }
]
~~~
