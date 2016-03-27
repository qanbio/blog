---
layout: post
title: Faire cohabiter plusieurs versions de Java sous une machine Linux
categories: [tech]
tags: [java, linux, debian, jdk, jre]
author: paterne_gaye
comments: true
---



Il peut arriver qu'on ait besoin d'avoir plusieurs versions de Java sur la même machine. C'est le cas lorsque vous installer un OS (Ubuntu) qui vient avec java 7 ou 6 alors que vous avez besoin de Java 8. Dans ce cas, vous devez pouvoir switcher aisément d'une version à l'autre en indiquant au système quelle doit être la version courante.

Dans le l'univers Linux, update-alternatives est une des solutions pour gérer les cas ou on a plusieurs versions d'un même logiciel sur une machine.

Pour rappel, au quotidien nous utilisons deux commandes Java :
* **java** qui permet d'exécuter un binaire (*.jar, .class* )
* **javac** qui permet de compiler un fichier source *.java*

Cette liste n'est pas exhaustive. Il existe d'autres commandes Java notament *javaws* . Pour chacune de ces commandes il faudra reprendre la procédure ci-dessous.

L'objectif de ce qui suit c'est que nous puissions exécuter toujours */usr/bin/javac* ou */usr/bin/java* sans nous soucier de l'emplacement exacte des exécutables ni de la leur versions.

{% highlight bash %}
sudo update-alternatives --install /usr/bin/javac javac /opt/jdk1.8.0_101/bin/javac 1
sudo update-alternatives --install /usr/bin/java java /opt/jdk1.8.0_101/bin/java 1
{% endhighlight %}

{% highlight bash %}
sudo update-alternatives --config javac
sudo update-alternatives --config java
{% endhighlight %}

