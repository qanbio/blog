---
layout: post
title: Installation de apache en ligne de commande sur une machine Linux
categories: [tech]
tags: [apache, linux, debian, installation,server, setup]
author: crepin_fadjo
comments: true
fullview: false
description: Ce post est un aide-mémoire pour l'installation en ligne de commande de apache
---
## Apache
>Le logiciel libre Apache HTTP Server (Apache) est un serveur HTTP créé et maintenu au sein de la fondation Apache. C'est le serveur HTTP le plus populaire du World Wide Web

Source : [wikipedia](https://fr.wikipedia.org/wiki/Apache_HTTP_Server)

### Installation apache

Avant toute installation sur linux,il faut mettre à jour le package avec la commande

{% highlight bash %}
sudo apt-get update
{% endhighlight bash %}

Maintenant on peut installer apache avec la commande

{% highlight bash %}
sudo apt-get install apache2
{% endhighlight bash %}

![installation apache](../../../../assets/media/2017-02-20-installation-lamp-linux/instal_apache.PNG "installation apache")

### Configuration
On peut configurer apache en modifiant son port de démarage et mmettant la valeur voulu.
Pour ce fait exécuter ouvrir le fichier /etc/apache2/port.conf et modifier la valeur du port

{% highlight bash %}
sudo vi /etc/apache2/ports.conf
{% endhighlight bash %}

![configuration port apache](../../../../assets/media/2017-02-20-installation-lamp-linux/port.PNG "configuration port apache")

Redemarrer apache
{% highlight bash %}
sudo /etc/init.d/apache2 restart
{% endhighlight bash %}

### Test

Maintenant nous pouvons tester apache en allant dans un navigateur et en allant sur  l’url suivante

{% highlight bash %}
<adresse IP>:<port>
{% endhighlight bash %}

![verification fonctionnement apache](../../../../assets/media/2017-02-20-installation-lamp-linux/apache.PNG "verification fonctionnement apache")