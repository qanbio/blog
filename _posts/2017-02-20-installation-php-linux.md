---
layout: post
title: "Mise en place d'un serveur RapidPro sur un serveur debian"
categories: [tech]
tags: [linux, debian, rapidpro] 
author: ginette_hounkponou
comments: true
fullview: false
description: Ce post est un aide-mémoire pour la mise en place du serveur rapidpro sur un serveur debian 
---
## PHP
>PHP: Hypertext Preprocessor, plus connu sous son sigle PHP (acronyme récursif), est un langage de programmation libre7, principalement utilisé pour produire des pages Web dynamiques via un serveur HTTP6, mais pouvant également fonctionner comme n'importe quel langage interprété de façon locale. PHP est un langage impératif orienté objet.

Source : [wikipedia](https://fr.wikipedia.org/wiki/PHP)

### Pré requis
Pour pouvoir utiliser php côté serveur dans le cadre du développement d'une webapp, il vous faudra un serveur Web capable d'interpréter PHP. C'est le cas du serveur apache dont l'installation est décrite [ici](http://blog.qanbio.com/tech/2017/02/20/installation-apache-linux.html)

### Installation php

Avant toute installation sur linux,il faut mettre à jour le package avec la commande

{% highlight bash %}
sudo apt-get update
{% endhighlight bash %}

Maintenant on peut installer php avec la commande

{% highlight bash %}
$ sudo apt-get install php5
{% endhighlight bash %}

![installation php](../../../../assets/media/2017-02-20-installation-lamp-linux/install_php.PNG "installation php")

### Tester PHP

Pour tester si php est bien installé,exécuter la commande suivant:

{% highlight bash %}
php -v
{% endhighlight bash %}

![php version](../../../../assets/media/2017-02-20-installation-lamp-linux/php_version.PNG "php version")
