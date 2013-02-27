---
date: '2013-02-28 09:00:00'
datefr: 28 Février 2013
layout: post
status: publish
title: Web Components - Observer les changements du DOM
img: /assets/img/posts/longuevue.jpg
tags: web-components html chrome dom observe mutations angularjs databinding
more : 100
preview: Cette dernière partie de la spécification des web components est sûrement celle qui va avoir le plus d'impact sur les performances de nos applications. Contrairement aux templates, shadow dom et éléments customs qui sont plus orientés pour améliorer la productivité des développeurs. Nous allons donc voir comment "observer notre application", observer le DOM ainsi que le model.
---

Cette dernière partie de la spécification des web components est sûrement celle qui va avoir le plus d'impact sur les performances de nos applications. Contrairement aux templates, shadow dom et éléments customs qui sont certainement plus orientés pour améliorer la productivité des développeurs. Nous allons donc voir comment "observer notre application", observer le DOM ainsi que le model.

### On fait comment de nos jours ?

Aujourd'hui, pour observer les changements du DOM, nous utilisons les `MutationEvent`. Par exemple, pour être notifié de l'ajout d'un élément au DOM, on ajoute un listener sur le document

<pre class="prettyprint" data-lang="javascript">
document.addEventListener('DOMNodeInserted', function(e) {
  console.log(e.target);
}, false);
</pre>

Cette façon de faire n'est pas performante. D'une part à cause de la propagation des évènements et de la nature asynchrone de cette méthode. Ensuite car ces évènements sont envoyés beaucoup trop souvent, à chaque changement, sans possibilité de grouper ceux-ci.

### Demain, avec les Web Components...

Demain, ce sera mieux, évidemment !

La spécification Web Components nous apporte les <strong>Mutation Observers</strong> qui vont nous permettre d'observer les changements du DOM, sans avoir recours à la programmation évènementiel. Concrètement, qu'est-ce que ça change ?

Les Mutations Observers ne sont notifiés qu'à la fin des modifications du DOM, lorsqu'il y en a plusieurs simultanément par exemple. Ce qui implique que ce que l'on va recevoir une liste de changements et non plus un seul. 

On crée tout d'abord l'observer, la fonction qui va être notifiée des changements et effectuera les actions voulues.

<pre class="prettyprint" data-lang="javascript">
var observer = new MutationObserver(function(mutations, observer) {
  mutations.forEach(function(record) {
    for (var i = 0, node; node = record.addedNodes[i]; i++) {
      console.log(node);
    }
  });
});
</pre>

Sur cet objet, on va ensuite appeler la méthode observe

* Le premier paramètre est l'élément du DOM que l'on souhaite observer.
* Le second paramètre est un object contenant un ensemble de propriétés pour l'observation

<pre class="prettyprint" data-lang="javascript">
observer.observe(el, {
  childList: true,    	// Être notifié des ajouts/suppressions de noeud
  subtree: true,      	// Observer également le subtree
  characterData: true,	// Inclure les changements de contenu
  attribute: true     	// Inclure les changements d'attribut dans le subtree
});
</pre>

Enfin, on peut choisir d'arrêter l'observation 
<pre class="prettyprint" data-lang="javascript">
observer.disconnect();
</pre>

### Observer les changements du modèle 

On peut observer les changements du DOM assez simplement et efficacement... pas mal ! Mais ce que je trouve encore plus sympathique, c'est la possibilité d'observer les changements sur des objets JavaScript.

Tout comme pour l'observation du DOM, une fonction va être notifiée des changements sur un objet. Pour chaque changement, plusieurs informations vont être disponibles

* `name` Qu'est-ce qui a changé ? 
* `type` Quelle est la nature du changement ?
* `oldValue` l'ancienne valeur de ce qui a changé 
* `object` l'object observé dans son état courant

<pre class="prettyprint" data-lang="javascript">
function observeChanges(changes) {
  console.log('== Callback ==');
  changes.forEach(function(change) {
    console.log('Ce qui a changé', change.name);
    console.log('La nature du changement', change.type);
    console.log('L''ancienne valeur', change.oldValue );
    console.log('La nouvelle valeur', change.object[change.name]);
  });
}
</pre>

Enfin, démarrer l'observation.

<pre class="prettyprint" data-lang="javascript">
var o = {};
Object.observe(o, observeChanges);
</pre>

### Tout ça pour ça ?

Concrètement, tous les développeurs ne vont pas avoir besoin des Observers. Cette fonctionnalité vise principalement les auteurs de framework, les développeurs d'extensions... Avec (si vous avez suivi) en ligne de mire le <strong>databinding</strong> et les <strong>MDV, Model-Driven-View</strong>

Voici un graphe que j'ai honteusement piqué sur [la présentation](http://html5-demos.appspot.com/static/webcomponents/index.html) d'Eric Bidelman (Si je retrouve la source originale, je la mettrai ici), montrant les performances constatés par l'équipe d'AngularJS avec dans un cas le databinding fait de façon "classique" et dans un deuxième cas, en utilisant les Observers (Disponibles sur un branche de dev de Chromium)

<img src="/assets/img/posts/domobserve/angularspeedup.png">

### The end 

Il s'agissait du dernier article de la série. J'espère avoir réussi à vous inciter à suivre de près cette spécification, qui est selon moi un grand pas en avant dans le monde du Web, comme peut l'être HTML5 aujourd'hui. 

<strong>Disclaimer</strong> : Tout ce que j'ai pu écrire dans ces articles ne sera peut-être plus vrai au moment où vous les lirez. La spécification est encore à l'état draft et donc susceptible de changer avant sa sortie officielle.

####Les autres articles de la série sur les WebComponents : 

* [Web Components - Les templates](/2013/02/18/web-components/)
* [Web Components - Le Shadow DOM](/2013/02/22/shadow-dom/)
* [Web Components - Custom Elements](/2013/02/26/custom-elements/)
* [Web Components - Observer les changements du DOM](/2013/02/28/dom-observe/)