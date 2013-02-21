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
[<< Précédent : Web Components - Les templates](http://julienvey.com/2013/02/18/web-components/)


Après avoir introduit les [Web Components](http://julienvey.com/2013/02/18/web-components/) et la première feature, les templates, je vais maintenant vous parler du Shadom DOM, qui va nous garantir l'encapsulation dans nos pages html.

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
  <em>#shadow-root</em>
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
  <em>#shadow-root</em>
    &lt;h2&gt;Bouh ! Je suis le shadow DOMMMM !&lt;/h2&gt;
    &lt;div&gt;et je viens de remplacer le contenu du noeud&lt;/div&gt;
&lt;/div&gt;
</pre>

Problème ! On remarque ici que tout le contenu de notre noeud "host" a été supprimé et remplacé par le Shadow DOM.
On voudrait bien garder notre contenu et l'insérer dans notre shadow DOM.
Pour cela...

### Shadow DOM insertion points

Les insertions points (ou points d'insertion en bon français) vont nous permette d'insérer le contenu de notre élément host au sein même de notre Shadow DOM.

#### Tout d'abord un exemple simple...

Pour définir des points d'insertion au sein de notre shadow DOM, nous allons utiliser la balise `<content>`

Imaginons que je souhaite utiliser le DOM suivant comme host...
<pre class="prettyprint" data-lang="html">
&lt;div id="host"&gt;
  &lt;div&gt;Hello World !&lt;/div&gt;
&lt;/div&gt;
</pre>

pour ce shadow DOM...

<pre class="prettyprint" data-lang="html">
&lt;div&gt;Je commence à apprendre les Web Components&lt;/div&gt;
<strong>&lt;content&gt;&lt;/content&gt;</strong>
&lt;div&gt;J'ai fini d'apprendre les Web Components&lt;/div&gt;
</pre>

Une fois le Shadow DOM injecté, le résultat est donc...

<pre class="prettyprint" data-lang="html">
&lt;div id="host"&gt;
  <em>#shadow-root</em>
    &lt;div&gt;Je commence à apprendre les Web Components&lt;/div&gt;
    <strong>&lt;div&gt;Hello World !&lt;/div&gt;</strong>
    &lt;div&gt;J'ai fini d'apprendre les Web Components&lt;/div&gt;
&lt;/div&gt;
</pre>

#### Utilisation des sélecteurs CSS 

Voyons maintenant un exemple un peu plus complexe. Nous allons utiliser l'attribut `select` pour définir des sélecteurs CSS afin de spécifier quels éléments doivent être insérés et où.

Voici un exemple de shadow DOM dans lequel on définit plusieurs points d'insertion en utilisant des sélecteurs 

<pre class="prettyprint" data-lang="html">
&lt;hgroup&gt;
  <strong>&lt;content select="h2"&gt;&lt;/content&gt;</strong>
  &lt;div&gt;Bouh ! Je suis le shadow DOMMMM !&lt;/div&gt;
  <strong>&lt;content select="h1:first-child"&gt;&lt;/content&gt;</strong>
&lt;/hgroup&gt;
<strong>&lt;content select="*"&gt;&lt;/content&gt;</strong>
</pre>

Si l'on reprend le DOM host original (avec Titre, Sous-titre et Contenu) et que l'on y injecte le shadow DOM, le résultat est le suivant 

<pre class="prettyprint" data-lang="html">
&lt;div id="host"&gt;
  <em>#shadow-root</em>
    &lt;hgroup&gt;
      <strong>&lt;h2&gt;Sous-titre&lt;/h2&gt;</strong>
      &lt;div&gt;Bouh ! Je suis le shadow DOMMMM !&lt;/div&gt;
      <strong>&lt;h1&gt;Titre&lt;/h1&gt;</strong>
    &lt;/hgroup&gt;
    <strong>&lt;div&gt;contenu&lt;/div&gt;</strong>
&lt;/div&gt;
</pre>

En plus de masquer l'implémentation interne des composants, le Shadow DOM nous permet donc de réordonner selon nous souhaits le DOM.


é
ê
è
ê
à
ù