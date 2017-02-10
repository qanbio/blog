---
layout: post
title: Gestion de plusieurs versions d'un même logiciel sous Linux
categories: [tech]
tags: [java, linux, debian, jdk, jre, version, multiversioning, setup]
author: paterne_gaye
comments: true
fullview: false
description: Ce post est un aide-mémoire pour la gestion de plusieurs versions d'un même logiciel sous Linux
---

Dans le l'univers Linux, update-alternatives est une des solutions pour gérer les cas ou on a plusieurs versions d'un même logiciel sur une machine.

Prenons le cas de Java ...

Todo : a aranger

Pour rappel, au quotidien nous utilisons deux commandes Java :

* **java** qui permet d'exécuter un binaire (*.jar, .class* )
* **javac** qui permet de compiler un fichier source *.java*

Cette liste n'est pas exhaustive. Il existe d'autres commandes Java notament *javaws* . Pour chacune de ces commandes il faudra reprendre la procédure ci-dessous.

L'objectif de ce qui suit c'est que nous puissions exécuter toujours */usr/bin/javac* ou */usr/bin/java* sans nous soucier de l'emplacement exacte des exécutables ni de la leur versions.Le chiffre *1* ci-dessus indique la priorité de la version.

{% highlight bash %}
sudo update-alternatives --install /usr/bin/javac javac /opt/jdk1.8.0_101/bin/javac 1
sudo update-alternatives --install /usr/bin/java java /opt/jdk1.8.0_101/bin/java 1
{% endhighlight %}

Vous pouvez maintenant indiquer au système quelle version vous souhaitez utiliser :
{% highlight bash %}
sudo update-alternatives --config javac
sudo update-alternatives --config java
{% endhighlight %}

A noter que si vous n'avez qu'une version disponible, cette commande sera sans effet.