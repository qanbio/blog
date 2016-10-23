---
layout: post
title: "Création d'un blog statique avec Jekyll et Github (1/2)"
categories: [tech]
tags: [jekyll, github, blog, markdown]
author: paterne_gaye
comments: true
description: Chez Qanbio, quand nous avons voulu créer notre blog, nous avions le choix entre plusieurs alternatives. Notamment les mastodontes : Blogger, Wordpress etc. Nous avons fini par opter pour un blog statique généré à l'aide de Jekyll et hébergé sur Github. Dans cet article nous allons présenter ce qui a motivé notre choix et la procédure pour développer un blog analogue.
---

# Pré-requis

## Markdown
Ce format, moins intrusif que le html, permet de mélanger la mise en forme et le texte tout en gardant un texte lisible pour l'humain. Le véritable point fort de Markdown, c'est sa légereté et sa simplicité. Par exemple dans Markdown, pour mettre un texte en italique on l'entour de deux caractères "*". Par ailleurs Mardawn a l'avantage de pouvoir être convertible en HTML. Certes Markdown n'a pas la puissance de html et ne prétend d'ailleurs pas le remplacer mais pour la rédactions d'un grand nombre de documents : commentaires, articles de blog, documentation utilisateurs ... Markdown suffit aisément et permet au rédacteur non informaticien de rédiger un document tout en spéciant la mise en forme.

## Git
Git est un système de gestion de fichiers. Grosso-modo, il permet à plusieurs personnes de collaborer sur les même fichiers en garantissant un historique des modifications effectuées. Git a été créé par Linux Torvald, le créateur du système d'exploitation Linux. Dans le monde open source, Git est aujourd'hui la référence dans le versionning du code code source.

## Github et Github page
Github est un service basé sur Git qui permet de stocker et de gérer le versionning de fichiers (du code source) dans le cloud.  Sous certaines conditions, notamment le volume des données stockées et le caractère privé de ces données, Github est gratuit ou payant.

Github offre une fonctionnalité appelée Github page qui permet à un abonné de github d'héberger gratuitement un site statique chez Github.

# Jekyll et Github en pratique

## Rédaction d'un article de blog
Dans un éditeur, en général notre IDE, nous créons un nouveau fichier yyyy-mm-dd-titre-de-l-article. Par exemple, cet article est rédigé dans un fichier appelé 2016-03-02-creation-blog-statique-jekyll-github. Voici à quoi ressemblent les premières lignes de ce fichier

{% highlight yaml %}
layout: post
title: Création d'un blog statique avec Jekyll et Github
categories: [tech]
tags: [jekyll, github, blog]
description: "Création d'un blog statique avec Jekyll et Github"
author: paterne_gaye
comments: true
{% endhighlight %}

* layout: template à appliquer pour la mise en forme.
* title: titre de l'article
* categories: les catégories dans lesquelles vous publiez, chez Qanbio, nous avons les catégories "opinion", "tech" et "business"
* tags: les mots clés, ceci falitera l'indexation et donc les recherches des utilisateurs
* description: Création d'un blog statique avec Jekyll et Github
* author: l'indentiant de l'auteur, les auteurs sont définis dans un fichier yaml
* comments: autorise t-on les commentaires sur cet articles

Le reste du fichier correspond au contenu del'article au format Markdown.

## Publication du blog
Lorsque nous rajoutons ou modifions un article sur notre post, nous validons nos modification à l'aide de Git vers Github. Presque immédiatement, notre blog est redéployé et nos modifications sont en ligne.

# Pourquoi le tandem Jekyll-Github?

## Motivations usuelles
Dans cet [article](http://tom.preston-werner.com/2008/11/17/blogging-like-a-hacker.html), [Tom Preston-Werner](https://en.wikipedia.org/wiki/Tom_Preston-Werner), entre autre inventeur de Gravatar et fondateur de Github, explique les raisons qui l'ont motivé à tourner le dos au Wordpress et autres pour créer Jekyll. Entre autres, il reproche aux CMS traditionnels, leur trop grande complexité comparée à la simplicité du besoin. Par exemple, il regrette l'obligation de devoir faire des mises à jour régulières lorsqu'on est sous Wordpress ou encore de devoir emabarquer une base de données alors qu'on ne manipule que des fichiers texte dans un blog.

En général, sur le principe même de choisir entre un site dynamique géré par un CMS et un site statique sur généré par Jekyll, il y a plusieurs arguments interessants tels que :
* Les site statiques sont moins vulnérables aux hacker
* Les sites statiques sont meilleurs quant à la scalabilité ce qui peut être utile si vous écrivez un article qui a du succès
* Vous payer juste pour l'hébergement et non plus pour des fonctionnalités qui ne vous servent à rien pour un blog, notamment la base de données.

Cependant, ce ne sont pas ces raisons qui motivé notre choix. Nous sommes moins interessé par les "défauts" des CMS que les avantages du tandem Jekyll-Github. Dans la suite, nous allons donc laisser tomber le débat *static vs dynamic* pour nous interesser aux avantanges de Jekyll couplé avec Github.

## Ce qui nous a motivé

### Bloger comme un hacker
Nous sommes des développeurs. Nous passons nos journées devant nos IDE Android studio, Webstorm et IntelliJ selon les projets sur lesquels nous travaillons. Plusieurs fois par jour nous *pullons* et *pushons* du code à l'aide de Git vers nos repositories sur bitbucket.org, un conccurrent de Github.

Avec le duo Jekyll-Github, finalement écrire un article de blog, c'est exactement le même processus que créer une classe Java :
* Depuis l'IDE, nous créons le fichier puis le modifions
* En ligne de commande ou depuis l'IDE selon le gout du jour, nous *comitons* et *pushons* nos modifications sur Github

Et c'est tout, un fois le *push* effectué, nous n'avons à nous préoccuper de rien : le blog est redéployé dans la minute qui suit.

Au fond, pour citer Tom Preston-Werner, nous souhaitions "bloger comme des hackers" et Jekyll couplé à Github nous permet celà.


### L'indépendance
Avec nos articles au format Markdown, nous pouvons sans effort, changer d'hébergeur et de technoogie. Par contre migrer des CMS tels que Wordpress n'est pas toujours aisé.

# Installation de Jekyll
To be continued ...

