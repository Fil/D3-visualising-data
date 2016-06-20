---
layout: page
title: Javascript
subtitle: Nourrir un chat
minutes: 20
---

> ## Objectifs de la leçon {.objectives}
>
> * Charger un fichier Javascript
> * Passer des éléments HTML à Javascript
> * Manipuler des éléments HTML avec Javascript

Nous avons appris à intégrer du texte et des éléments graphiques dans notre page, et nous avons aussi appris à publier cette page.
Jusqu’ici, rien de ce que nous avons fait n’est spécifique à la visualisation de données. Nous pourrions très bien créer notre graphique dans un autre logiciel, l’exporter sous la forme d’une image, et le publier comme cela. Mais cette approche ne nous permettra pas d’aller plus loin : comment permettre à l’utilisateur d’interagir avec les données ? Pour cela, il va falloir apprendre à écrire des scripts. À nouveau, HTML fournit la solution en permettant d’entrer des scripts.
Tout ce qui est défini entre les balises &lt;script&gt; et &lt;/script&gt; dans l’entête ou le corps de la page est interprété comme du code Javascript. Puisque le code est exécuté au moment où il apparaît dans la page, il faut s’assurer que les éléments auxquels on va faire référence existent déjà. La manière la plus simple de procéder est d’écrire les scripts vers le bas de la page, juste avant la balise </body> fermante. 
Comme nous l’avons fait avec les styles, nous pouvons exporter notre code dans un fichier externe, cette fois-ci avec l’extension `.js`. 

Revenons maintenant à notre image de chat. Nous allons lui demander de miauler quand l’utilisateur le caresse avec le bout de sa souris.
Il faut tout d’abord créer le fichier `interaction.js` , et l’appeler dans le corps du fichier HTML.

~~~{.html}
<script src="interaction.js"></script>
~~~

Pour que le script puisse fonctionner avec la page HTML, nous avons quatre étapes à formuler:

* Créer un lien entre le HTML et le script — par exemple en donnant un identifiant unique à l’image avec laquelle nous voulons interagir.
* Récupérer l’élément en question, et le mémoriser dans une variable du script, pour pouvoir travailler dessus.
* Détecter l’action qui nous intéresse — c’est-à-dire le clic de souris, en utilisant un « event listener ».
* Réaliser une action.

L’identifiant (ID) est un attribut unique que l’on peut donner à l’image sous cette forme:

~~~{.html}
<div class='image'>
	<img id="cat" src="img/cat.jpg">
</div>
~~~

La fonction Javascript `getElementById` nous permet de récupérer cet élément depuis le `document` (un objet magique qui représente la page entière), et le mémoriser dans une variable pour travailler ensuite avec depuis le fichier Javascript.

~~~{.js}
var cat_image = document.getElementById('cat');
~~~

Nous voulons maintenant détecter si quelqu’un a cliqué sur l’image du chat.
Les « listeners » nous aident en nous indiquant les événements réalisés par les humains sur notre page.

~~~{.js}
var cat_image = document.getElementById('cat');
cat_image.addEventListener("click", meow);
~~~

Notre listener prend deux arguments: le type d’événement que l’on va écouter, et une fonction dite « callback », qui sera appelée lorsque l’événement se produira. 
Cette fonction, nous l’appelons `meow()` (miaou), elle ouvrira une fenêtre pop-up. Pour cela nous pouvons utiliser la fonction Javascript alert().

~~~{.js}
function meow() {
	alert("Miaou!");
};
~~~

On peut aussi définir une fonction directement dans le listener, par exemple dans le cas où l’on veut exécuter une série d’opérations spécifiques à cet endroit. Ainsi par exemple la construction suivante va appeler la fonction `meow()`, mais aussi d’autres fonctions, comme `purr()` (ronronner… laquelle n’existe pas encore).

~~~{.js}
var cat_image = document.getElementById('cat');
cat_image.addEventListener("click", function() {
	meow();	
	purr();
});
~~~

On peut aussi tout à fait se passer de nommer les fonctions de callback, et les définir de façon entièrement anonyme:

~~~{.js}
var cat_image = document.getElementById('cat');
cat_image.addEventListener("click", function() {
	alert("Meow!");	
	purr();
});
~~~


> ## Débuguer dans le navigateur {.callout}
> Si l’on fait un clic-droit sur la page pour ouvrir l’inspecteur, on revient aux outils développeurs.
> Parmi les onglets présents se trouvent:
>
> * Console - La console nous prévient des erreurs. Elle permet aussi à notre code d’envoyer des messages, et d’afficher le contenu d’une variable `x`, en utilisant l’instruction `console.log(x)`.
> * Elements - Permet de vérifier si tous nos éléments HTML sont au bon endroit. En survolant n’importe quelle région de la page, on voit se surligner les éléments correspondants et on peut vérifier quels styles et attributs leur sont appliqués — et même modifier temporairement ces attributs, pour faire des essais « en direct ». 
> * Sources - Ceci nous donne accès à l’ensemble des fichiers qui définissent notre page. Pour les fichiers Javascript, on peut même positionner des _breakpoints_ pour mettre en pause l’exécution des scripts et observer l’état des variables.

> ## Nourrir votre chat {.challenge}
> Créer un bouton en utilisant l’élément &lt;button&gt;;  le bouton permettra de nourrir le chat en utilisant les éléments définis plus haut.
> Utiliser la fonction alert() pour que le chat vous remercie.

L’étape suivante pour une page totalement interactive, consiste à modifier les éléments HTML en utilisant Javascript. Nous avons créé un bouton dans le HTML, et ce bouton appelle une fonction quand il est cliqué. 

Le but maintenant est que le chat grossisse un peu à chaque fois qu’il est nourri. Pour cela il faut mémoriser à la fois le bouton et le chat.

Pour fixer la largeur de l’image, on va écrire ces mots, en les reliant par des points:
'cat_image.style.width'.
Il faut penser que la variable 'cat_image' est un objet possédant un attribut style, lequel à son tour a un attribut 'width'.
Ajoutons quelques grammes à notre chat.
Il nous faut d’abord récupérer la largeur courante de l’objet. Comme on peut le voir dans l’inspecteur, notre objet 'cat_image' a un attribut 'offsetWidth'. Ceci nous donne sa largeur courante sous forme d’un nombre (et non pas d’une chaîne '200px').
On va calculer la nouvelle valeur, et lui ajouter 'px’, puis l’indiquer dans le style de notre image.

~~~{.js}
var cat_image = document.getElementById('cat');
var feed_button = document.getElementById('button1');

button1.addEventListener("click", nourrir_le_chat);
function nourrir_le_chat() {
	cat_image.style.width = (cat_image.offsetWidth + 30.0) + 'px';
};
~~~

On peut aussi passer un argument à la fonction `nourrir_le_chat` en l’inscrivant dans les parenthèses.
Par exemple, l’utilisateur pourrait chisir quelle portion de nourriture il donne à son chat.

Il faudrait alors renvoyer une valeur et l’assigner à une variable (tout comme on a fait avec n’importe quelle autre fonction, par exemple `var element = document.getElementById("someID");`): 

~~~{.js}
var new_width = feed(10);

function feed(mealsize) {
		cat_image.style.width = (cat_image.offsetWidth + parseInt(mealsize)) + 'px';
		return cat_image.style.width;
};
~~~

En Javascript il y a deux grands types de données: les chaînes de caractères (texte, tout entre guillemets)
et les nombres. Il est important de se souvenir qu’on ne peut pas faire de calculs avec des chaînes.

Par exemple:
`5+5 = 10`
mais
`'5'+'5' = '55'`

Si l’un des arguments est une chaîne, l’autre sera converti, par exemple:
`5 + '5' = '55'`

C’est ce que l’on vient de faire lorsque nous avons concaténé `(cat_image.offsetWidth + 30.0) + 'px'`.

> ## Autres listeners qui peuvent s’avérer utiles  {.callout}
> * dblclick - Double clic
> * contextmenu - Clic-droit
> * mouseover - Le pointeur de la souris passe sur un élément
> * keypress - Appui d’une touche du clavier 


> ## Un peu de sport pour notre chat  {.challenge}
> Créer un second bouton “faire du sport” qui fasse maigrir le chat.


À la fin de cette leçon, votre page devrait ressembler à ceci:

<iframe src="http://isakiko.github.io/D3-visualising-data/code/meow.html" width="1000" height="250"></iframe>

