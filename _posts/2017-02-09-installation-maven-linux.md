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

# Présentation de Maven

Maven est un outil multifonctions et une des manière les plus simple de l'appréhender est sans doute d'introduire ses fonctionnalités.

## Fonctionnalités de Maven

### Notion de gestion de dépendance
La gestion de dépendance est un mécanisme de centralisation des informations de dépendance. Lorsque vous avez un ensemble de projets qui hérite d'un parent commun, il est possible de mettre toutes les informations sur la dépendance dans le POM commun et d'avoir des références plus simples aux artefacts dans les POM enfant.


## Notion de d'Archetype
Un archétype est un modèle de projet. Maven propose en standard plusieurs :En voici quelques exemples

|          Archétype            |                        Description                            |
| ----------------------------- |---------------------------------------------------------------|
| maven-archetype-archetype     | Un archétype pour un exemple d'archétype                      |
| maven-archetype-j2ee-simple   | Un archétype pour un projet de type application J2EE simplifié|
| maven-archetype-mojo          | Un archétype pour un exemple de plugin Maven                  |




D'autres achétypes peuvent être proposés par des tiers. Il est aussi possible de développer ses propres archétypes

### Notion de gestionnaire de build
Un gestionnaire de build est un outil qui permet d'éxecuter un script qui contient une suite d'objectifs souvent appelés cibles ou "target". Chaque target a un rôle bien particulier par rapport au code qu'il accompagne. Certain target prépare l'environnement de compilation, d'autre le compile ou encore le nettoie. On peut aussi générer la documentation à partir des commentaires du code. Les outils les plus répandus sont ceux de la fondation Apache tels que : MAVEN

## Alternatives de Maven
* Gradle le dernier et sans doute le concurrent le plus sérieux
* Ant, dont Maven est "le successeur" mais qui est fonctionnellement beaucoup plus limité
* Ivy,

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

