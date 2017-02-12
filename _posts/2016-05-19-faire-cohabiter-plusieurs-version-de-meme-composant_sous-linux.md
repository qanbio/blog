---
layout: post
title: Gestion de plusieurs versions d'un même logiciel sous Linux : Cas de Java
categories: [tech]
tags: [java, linux, debian, jdk, jre, version, multiversioning, setup]
author: paterne_gaye
comments: true
fullview: false
description: Ce post est un aide-mémoire pour la gestion de plusieurs versions d'un même logiciel sous Linux, en particulier Java
---

# Le problème
Il arrive qu'on doive gérer plusieurs logiciels se rapportant à une même fonctionnalité. Parfois il s'agit de logiciels completement différents, par exemple, on peut avoir plusieurs éditeurs de texte développés par des compagnies conccurentes. Il y a aussi des cas ou il s'agit de plusieurs versions du même logiciel, par exemple Java 7 et Java 8.

A chaque fois, le problème est le même : lorsqu'on switche d'un logiciel à un autre, on voudrais garder la même commande même si le logiciel a changé.

Prenons le cas de Java. Au quotidien nous utilisons deux commandes :

* **java** qui permet d'exécuter un binaire (*.jar, .class* )
* **javac** qui permet de compiler un fichier source *.java*

En pratique, les commandes *java* et *javac* ainsi lancées correspondent aux exécutables situés dans des dossiers précis déclarés au niveau de la variable PATH.

Lorsqu'on passe d'un version de Java à une autre, il faut s'assurer que les exécutables java et javac sont bien ceux correspondant à la version actuelle de Java


# La solution
La solution *naturelle* est de modifier les variables d'environnement, notament JAVA_HOME et PATH cependant cette solution nous parait trop conraignante et nous lui préférons l'outil update-alternatives.

Grosso modo, cette solution consiste à gérer les versions à l'aide de liens symboliques. Ainsi, pour le cas de Java, nous créons des liens dans */usr/bin/java* et */usr/bin/javac* qui pointent vers les exécutables souhaités. lorsqu'on switche d'une version à une autre, il suffit de changer le lien symbolique ce qui est transparent pour les utilisateurs.

# Usage de update-alternatives
L'objectif de ce qui suit c'est que nous puissions exécuter toujours */usr/bin/javac* ou */usr/bin/java* sans nous soucier de l'emplacement exacte des exécutables ni de la leur versions.Le chiffre *1* ci-dessus indique la priorité de la version.

Le script qui suit est à exécuter pour toutes versions de Java disponibles.
{% highlight bash %}
sudo update-alternatives --install /usr/bin/javac javac /opt/jdk1.8.0_101/bin/javac 1
sudo update-alternatives --install /usr/bin/java java /opt/jdk1.8.0_101/bin/java 1
{% endhighlight %}

Nous pouvons maintenant indiquer au système quelle version nous souhaitons utiliser :
{% highlight bash %}
sudo update-alternatives --config javac
sudo update-alternatives --config java
{% endhighlight %}

A noter que si vous n'avez qu'une version disponible, cette commande sera sans effet.