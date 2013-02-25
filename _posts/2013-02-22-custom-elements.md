---
date: '2013-02-25 09:00:00'
datefr: 25 Février 2013
layout: post
status: publish
title: Web Components - Custom Elements
img: /assets/img/posts/brick.png
tags: web-components html chrome dom custom-elements
more : 100
preview: Maintenant que nous connaissons les templates et que nous avons savons comment maîtriser le Shadow DOM, si on mélangeait les deux ? Enrichissons le DOM avec nos propres éléments
---

Maintenant que nous connaissons [les templates](/2013/02/18/web-components/) et que nous avons savons comment maîtriser [le Shadow DOM](2013/02/22/shadow-dom/), si on mélangeait les deux ?

### Enrichir le DOM avec ses propres éléments

En lisant les articles précédents, certains m'ont dit "Pour faire un template, tu utilises la balise template... pfff... normal quoi !"

Maintenant que je souhaite faire un élément, je vais utiliser quelle balise selon vous ? 

<pre class="prettyprint" data-lang="html">
&lt;element&gt;
</pre>

pfff... trop facile !

Et si on utilisait tout ce qu'on a appris jusqu'à maintenant ?

Je veux donc un élément qui possède un template, dont l'implémentation est masquée, dans lequel je pourrais insérer des éléments là où je l'ai défini, avec un style juste pour lui, et avec un peu de comportement si possible.

<pre class="prettyprint" data-lang="html">
<strong>&lt;element name="x-helloworld" constructor="HelloWorld"&gt;</strong>
  <strong>&lt;template&gt;</strong>
    &lt;style&gt;
      /* Le style pour cet élément */
    &lt;/style&gt;
    Hello <strong>&lt;content&gt;&lt;/content&gt;</strong> ! I'm a custom element !
  <strong>&lt;/template&gt;</strong>
  &lt;script&gt;
    HelloWorld.prototype = {
      sayHello: function(e) {
        //do something
      }
    };
  &lt;/script&gt;
&lt;/element&gt;
</pre>

Ce composant est défini dans son propre fichier html

Pour l'utiliser dans sa propre page html, il faut maintenant importer ce fichier en utilisant un nouveau lien de type `component` puis ensuite utiliser la nouvelle balise disponible `<x-helloworld>`

<pre class="prettyprint" data-lang="html">
&lt;link rel="components" href="x-helloworld.html"&gt;
&lt;x-helloworld&gt;Julien&lt;/x-helloworld&gt;
</pre>

Ce composant va donc nous afficher ...

<strong>Hello Julien ! I'm a custom element !</strong>

Simple mais on imagine facilement toutes les possibilités que cela nous ouvre

### L'héritage

Dans l'exemple précédent, j'ai ajouté du comportement à mon élément en fournissant une "API" via la balise `<script>` interne à mon composant.

Mais lorsque l'on souhaite simplement fournir un nouvel élément basé sur un élément existant du DOM, on peut choisir d'hériter de celui-ci.

<pre class="prettyprint" data-lang="html">
&lt;element name="x-superbutton" extends="button" &gt;
  ...
&lt;/element&gt;
</pre>

### C'est déjà fini ?

Et oui, un article court mais qui a selon moi le mérite de mettre en valeur les deux précédents.

Mais il nous reste encore une dernière feature intéressante à voir. Comment observer les changements du DOM avec l'API Observer (Coming soon)

####Les autres articles de la série sur les WebComponents : 

* [Web Components - Les templates](/2013/02/18/web-components/)
* [Web Components - Le Shadow DOM](/2013/02/22/shadow-dom/)
* [Web Components - Custom Elements](/2013/02/25/custom-elements/)