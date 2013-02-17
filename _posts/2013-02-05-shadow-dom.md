---
date: '2013-02-20 09:00:00'
datefr: 20 Février 2013
layout: post
status: publish
title: Web Components - Le Shadow DOM
img: /assets/img/posts/ghostbuster-150.png
tags: web-components html chrome shadow-dom dom
more : 100
preview: Après avoir introduit les Web Components et la première feature, les templates, je vais maintenant vous parler du Shadom DOM, qui va nous garantir l'encapsulation dans nos pages html.
---
Après avoir introduit les Web Components et la première feature, les templates, je vais maintenant vous parler du Shadom DOM, qui va nous garantir l'encapsulation dans nos pages html.

###Le shadow DOM... mais il est où ?

####L'encapsulation d'aujourd'hui... en quelque sorte :(
<pre class="prettyprint" data-lang="html">
&lt;iframe&gt;
</pre>

Les iframes nous offrent un certain niveau d'encapsulation dans nos applications web mais restent trop contraignantes et problématiques pour être utilisées à grande échelle

####L'encapsulation de demain
Le principe du Shadow DOM est de masquer en partie le DOM interne d'un composant. Afin de laisser la responsabilité de l'implémentation d'un composant à la personne qui le fournit.

En fouillant un peu dans le DOM, on se rend compte que certains éléments masquent déjà leur implémentation interne

Ainsi, un lecteur vidéo que l'on voit ainsi...

<img src="/assets/img/posts/webcomponents/videocontrol.png">

...et représenté ainsi dans le DOM...
<pre class="prettyprint" data-lang="html">
&lt;video controls src="/ma/super/video"&gt;&lt;/video&gt;
</pre>

...se révèle beaucoup plus complexe lorsque l'on active l'affichage du shadow DOM (Option disponible dans Chrome Developer Tools)

<pre class="prettyprint" data-lang="html">
&lt;video controls="" height="300" src="/ma/super/video"&gt;
  #shadow-root
    &lt;div&gt;
      &lt;div&gt;
        &lt;div&gt;
    	  &lt;input type="button"&gt;
    	  &lt;input type="range" precision="float" max="596.48"&gt;
    	  &lt;div style="display: none;"&gt;0:00&lt;/div&gt;&lt;div&gt;9:56&lt;/div&gt;
    	  &lt;input type="button"&gt;
    	  &lt;input type="range" precision="float" max="1" style=""&gt;
    	  &lt;input type="button" style="display: none;"&gt;
    	  &lt;input type="button" style=""&gt;
    	&lt;/div&gt;
      &lt;/div&gt;
    &lt;/div&gt;
&lt;/video&gt;
</pre>

Le Shadow DOM va donc nous permettre de mettre en place les mêmes mécanismes que ceux qui sont déjà utilisés par les navigateurs pour implémenter les composants natifs.

#### Comment créer du Shadow DOM

Pour utiliser le shadow DOM, il va vous falloir un élément "host", qui va accueillir notre shadow DOM, commme peut l'être la balise vidéo dans l'exemple précédent

En partant du DOM suivant...
<pre class="prettyprint" data-lang="html">
&lt;div id="host"&gt;
  &lt;h1&gt;Titre&lt;/h1&gt;
  &lt;h2&gt;Sous-titre&lt;/h2&gt;
  &lt;div&gt;contenu&lt;/div&gt;
&lt;/div&gt;
</pre>

... on peut créer du shadow DOM via JavaScript

<pre class="prettyprint" data-lang="javascript">
var host = document.querySelector('#host');
var shadow = host.createShadowRoot();
shadow.innerHTML = '&lt;h2&gt;Bouh ! Je suis le shadow DOMMMM !&lt;/h2&gt;' +
                   '&lt;div&gt;et je viens de remplacer le contenu du noeud&lt;/div&gt;';
</pre>

Ce qui donne comme résultat :

<pre class="prettyprint" data-lang="html">
&lt;div id="host"&gt;
  #shadow-root
    &lt;h2&gt;Bouh ! Je suis le shadow DOMMMM !&lt;/h2&gt;
    &lt;div&gt;et je viens de remplacer le contenu du noeud&lt;/div&gt;
&lt;/div&gt;
</pre>

Problème ! On remarque ici que tout le contenu de notre noeud "host" a été supprimé et remplacé par le Shadow DOM.

### Quelques resources 
- La présentation [&lt;web&gt;components&lt;/web&gt;](http://html5-demos.appspot.com/static/webcomponents/index.html#2) de Eric Bidelman, Google Chrome Team, dont je me suis inspiré pour écrire cet article
- La spécification W3C [Web Components](https://dvcs.w3.org/hg/webcomponents/raw-file/tip/explainer/index.html)
- La page Google+ [Web Components](https://plus.google.com/103330502635338602217/posts)

é
ê
è
ê
à
ù