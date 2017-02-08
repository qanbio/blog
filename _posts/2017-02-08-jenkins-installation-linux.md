---
layout: post
title: Installation de Jenkins sur une machine Linux
categories: [tech]
tags: [linux, debian, jenkins, hudson, ci, ic, "integration continue", "continuous integration" ]
author: paterne_gaye
comments: true
fullview: false
description: Ce post est un aide-mémoire pour l'installation et la configuration basique de Jenkins sur une machine Linux Debian.
---

# Qu'est ce que Jenkins ?
Jenkins est un outil open source d'intégration continue (Lien vers intégration continue sur Wikipedia) développé en Java.

# Installation de Jenkins
Jenkins est une application web Java et pour l'installation, il est possible de la déployer sur un container de servlet tel que Apache Tomcat. Mais il est également possible d'installer Jenkins de manière autonome sous forme de service Linux. C'est cette dernière solution que nous avons adoptée car nous paraissant plus simple et efficace.


## Configurer la communication entre notre machine Linux et le repo de Jenkins
{% highlight bash %}
#Todo : insérer une commande ici
{% endhighlight %}

## Ajouter le repo de Jenins dans la liste des repo de notre machine Linux
{% highlight bash %}
#Todo : insérer une commande ici
{% endhighlight %}

## Mettre à jour le système et installer le "paquet jenkins"
Todo : insérer une commande ici


# Configuration de Jenkins

## Ajout des comptes utilisateurs
Todo : captures d'écran

## Modification du port
{% highlight bash %}
#Todo : insérer une commande ici
{% endhighlight %}

## Authentification sur le repository
Chez QANBIO, nos repos sont hébergés sur Bitbucket. Il s'agit donc de mettre au user Jenkins depuis la machine de CI cloner le projet à chaque build. Cette opération se passe en deux temps :
* Génération des clés SSH pour le user Jenkins
* Ajout de la clé publique du user Jenkins sur Bitbucket
* Configuration de Jenkins pour lui indiquer avec quelle clé privée il doit communiquer avec Bitbucket

### Générations des clés SSH
{% highlight bash %}
#Todo : insérer une commande ici
{% endhighlight %}

### Ajout de la clé publique du user Jenkins sur Bitbucket
{% highlight bash %}
#Todo : insérer une commande ici
{% endhighlight %}

### Configuration de Jenkins pour lui indiquer avec quelle clé privée il doit communiquer avec Bitbucket
{% highlight bash %}
#Todo : insérer une commande ici
{% endhighlight %}


# Tester la configuration : création d'un Job qui build et teste notre projet

Todo : captures d'écran
