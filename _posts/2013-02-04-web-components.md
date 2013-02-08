---
date: '2013-02-04 09:00:00'
datefr: 4 Février 2013
layout: post
status: publish
title: Web Components, le nouvel espoir des développeurs Web 
img: ./assets/img/posts/webcomponents-150.png
tags: web-components html chrome
more : 100
preview: Lorsque l'on développe une application, plus on écrit de code, plus celui-ci a besoin d'être modulaire, réutilisable et encapsulé. Côté Backend, nous disposons déjà de tous les outils necéssaires. Quand on est développeur Web, on se rend vite compte que JavaScript et HTML ne nous facilitent pas la tâche. Mais heureusement, les gens du web ont décidé de ne pas nous laisser en plan
---

Lorsque l'on développe une application, plus on écrit de code, plus celui-ci a besoin d'être modulaire, réutilisable et encapsulé. Côté Backend, nous disposons déjà de tous les outils necéssaires. Quand on est développeur Web, on se rend vite compte que JavaScript et HTML ne nous facilitent pas la tâche.

Mais heureusement, les gens du web ont décidé de ne pas nous laisser en plan, et de nous donner un petit coup de main. Google, Mozilla, Microsoft & Co penchent actuellement sur une spécification pour faciliter l'isolation, l'encapsulation, la définition d'éléments customs... nativement dans nos navigateurs. **Les Web Components**

###Web Components ? Kesako !?

Il s'agit avant tout d'[une spécification du W3C](https://dvcs.w3.org/hg/webcomponents/raw-file/tip/explainer/index.html) en cours de rédaction mise en avant par les acteurs du Web (Les deux éditeurs de la spécification travaillent chez Google).

Mais en vrai ça consiste en quoi ? En vrai, les Web Components c'est :

* Templates
* Éléments customs
* Shadow DOM
 
Voilà pour le principal, mais c'est aussi 

* Encapsulation de style
* Observers (pour le modèle ainsi que pour le DOM)
* Variables CSS

###Les Templates... retour vers le futur

Avant de voir ce que nous allons pouvoir faire grâce aux Web Components, regardons déjà ce que nous faisons de nos jours.

####Les Templates d'aujourd'hui
1 - Tu me vois, tu me vois pas... 

<pre class="prettyprint" data-lang="html">
&lt;div id="montemplate" hidden&gt;
	&lt;img src="logo.png"&gt;
	&lt;div class="comment"&gt;&lt;/div&gt;
&lt;/div&gt;
</pre>
L'un des avantages de cette méthode est que l'on manipule bel et bien du DOM. En revanche toutes les ressources déclarées dans ce template seront chargées dès le départ même si le template n'est pas utilisé. De plus, tout ce qui concerne le style est rendu difficile. Le scope CSS doit être correctement défini mais cela n'empêche pas les possibles collisions de nommage.

2 - `<script>` être ou ne pas être...

La deuxième méthode consiste à surcharger la balise script pour y "stocker" le template que l'on souhaite utiliser pour par la suite l'injecter dans le innerHTML d'un élément HTML. En faisant cela, on encourage le parsing de chaînes de caractères au runtime

<pre class="prettyprint" data-lang="html">
&lt;script id="montemplate" type="text/x-handlebars-template"&gt;
  &lt;img src="logo.png"&gt;
  &lt;div class="comment"&gt;&lt;/div&gt;
&lt;/script&gt;
</pre>

####Les Templates de demain

A l'aide de la nouvelle balise `<template>` (qui porte quand même bien son nom), nous allons definir un template HTML. Les avantages ? On travaille directement avec le DOM. Le template est parsé mais n'est pas affiché, les `<script>` ne sont pas exécutés, les images ne sont pas chargées et les sons ou vidéos ne sont pas joués.

<pre class="prettyprint" data-lang="html">
&lt;template id="montemplate"&gt;
  &lt;img src="logo.png"&gt;
  &lt;div class="comment"&gt;&lt;/div&gt;
&lt;/template&gt;
</pre>

Et maintenant pour instancier ce template ? rien de plus simple !

<pre class="prettyprint" data-lang="javascript">
var t = document.querySelector('#montemplate');
document.body.appendChild(<strong>t.content.cloneNode(true)</strong>);
</pre>

Le fait d'ajouter le template au DOM avec `.appendChild` va déclencher le chargement de l'élément. les `<script>` vont être exécutés, les images vont être chargées et les sons ou vidéos vont être joués.

On peut également accéder au contenu de l'élément.

<pre class="prettyprint" data-lang="javascript">
t.content.querySelector('img').src = '/mon/path/vers/mon/image';
</pre>

On voit donc que l'attribut `.content` nous donne accès au "corps" du template et va nous permettre de le manipuler, y ajouter d'autres éléments, positionner des attributs... Voici l'interface du DOM de l'élément `<template>`

<pre class="prettyprint" data-lang="javascript">
interface HTMLTemplateElement : HTMLElement {
    readonly attribute DocumentFragment content;
}
</pre>

###Le shadow DOM... mais il est où ?

####L'encapsulation d'aujourd'hui... en quelque sorte :(
<pre class="prettyprint" data-lang="html">
&lt;iframe&gt;
</pre>

Les iframes nous offrent un certain niveau d'encapsulation dans nos applications web mais restent trop contraignantes et problématiques pour être utilisées à grande échelle

####L'encapsulation de demain
Le principe du Shadow DOM est de masquer en partie le DOM interne d'un composant. Afin de laisser la responsabilité de l'implémentation d'un composant à la personne qui le fournit.

En fouillant un peu dans le DOM, on se rend compte que certains éléments masquent déjà leur implémentation interne

<pre class="prettyprint" data-lang="html">
&lt;input type="date"&gt;

&lt;audio controls src="/ma/super/chanson"&gt;&lt;/audio&gt;
</pre>

Le Shadow DOM va donc nous permettre de mettre en place les mêmes mécanismes que ceux qui sont déjà utilisés par les navigateurs pour implémenter les balises natives.

Resources :
TODO : Ajout la prez de @ebidel


é
ê
è
ê
à
ù