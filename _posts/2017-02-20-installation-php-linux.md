---
layout: post
title: Installation de php en ligne de commande sur une machine Linux
categories: [tech]
tags: [php, linux, debian, installation, setup]
author: crepin_fadjo
comments: true
fullview: false
description: Ce post est un aide-mémoire pour l'installation en ligne de commande de php
---
## PHP
>PHP: Hypertext Preprocessor, plus connu sous son sigle PHP (acronyme récursif), est un langage de programmation libre7, principalement utilisé pour produire des pages Web dynamiques via un serveur HTTP6, mais pouvant également fonctionner comme n'importe quel langage interprété de façon locale. PHP est un langage impératif orienté objet.

Source : [wikipedia](https://fr.wikipedia.org/wiki/PHP)

### Pré requis
Pour pouvoir exécuter php dans un navigateur il faudrait l'installation d'un serveur web comme apache.Vous avez un article sur son installation [ici]()

### Installation php

Avant toute installation sur linux,il faut mettre à jour le package avec la commande

{% highlight bash %}
sudo apt-get update
{% endhighlight bash %}

Maintenant on peut installer php avec la commande

{% highlight bash %}
$ sudo apt-get install php5
{% endhighlight bash %}

### Tester PHP

Créons un fichier de test

{% highlight bash %}
sudo sh -c 'echo "<?php phpinfo();?>" > /var/www/html/phpinfo.php'
{% endhighlight bash %}


Maintenant ouvrez un navigateur et aller sur l’url suivante:

{% highlight bash %}
<adresse IP>:<port>/phpinfo.php
{% endhighlight bash %}

Vous devez avoir ceci

![verification fonctionnement php](../../../../assets/media/2017-02-20-installation-lamp-linux/php.PNG "verification fonctionnement php")
