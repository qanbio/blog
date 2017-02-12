---
layout: post
title: Installation de Apache Maven sous Linux
categories: [tech]
tags: [linux, debian, maven, build]
author: paterne_gaye
comments: true
fullview: false
description: Ce post est un aide-mémoire pour l'installation de Apache Maven sur une machine Linux.
---

# Pré-requis

## Maven
Maven a besoin de Java pour fonctionner. Il faut donc s'assurer que Java est (correctement installé) lien vers article correspondant  et que le variable d'environnement (Todo lien vers article correspondant ) *JAVA_HOME* est définie.

## Identifier les URLs de téléchargement

Les liens de téléchargements sont disponible sici : http://maven.apache.org/download.cgi

Dans notre cas, nous avons opté pour http://www-us.apache.org/dist/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz qui nous permet de télécharger les binaires de la version 3.3.9 archivés et compressés au format tar.gz

# Installation

Avant tout positionnez vous dans votre dossier *home* en exécutant la commande suivante

{% highlight shell %}
cd
{% endhighlight %}

## Téléchargement de l'archive

{% highlight bash %}
wget http://www-us.apache.org/dist/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz
{% endhighlight %}

A la fin de cette étape, vous aurez un fichier **apache-maven-3.3.9-bin.tar.gz** . Vérifier ceci en exécutant la commande :

{% highlight bash %}
ls -la
{% endhighlight %}

## Extraire les fichiers de l'archive
Nous allons extraire l'archive dans le dossier /opt  qui est historiquement réservé aux packages "optionnels", c'est à dire ne figurant pas dans les logiciels "par défaut" installés.

{% highlight bash %}
sudo tar zxvf apache-maven-3.3.9-bin.tar.gz -C /opt
{% endhighlight %}

A l'issue de cette étape, vous devriez avoir un nouveau répertoire dans le dossier */opt* : **apache-maven-3.3.9*

Vérifier ceci en exécutant la commande :
{% highlight bash %}
ls -la
{% endhighlight %}

A cette étape, Maven est installé. Vérifier ceci en exécutant la commande :

{% highlight bash %}
/opt/apache-maven-3.3.9/bin/mvn -v
{% endhighlight %}

## Rajouter Maven dans le PATH

{% highlight bash %}
sudo export M2_HOME=/opt/apache-maven-3.3.9
sudo export PATH=$M2_HOME/bin:$PATH
{% endhighlight %}

Nous pouvons donc désormais saisir directement *mvn* en ligne de commande sans avoir à préciser le chemin de l'installation.

{% highlight bash %}
mvn -v
{% endhighlight %}

