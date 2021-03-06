---
layout: post
title: Les variables d'environnement sur une machine Linux
categories: [tech]
tags: [linux, debian, env, var, variable, environnement]
author: crepin_fadjo
comments: true
fullview: false
description: Ce post est un aide-mémoire pour la création de variables d'environnement sous linux.
---

# Notion de variable d’environnement

Une variable d’environnement est une variable dynamique utilisée par des processus, y compris  les applications informatiques.  L'intérêt des variables d’environnement est de permettre à des processus isolés de manipuler la même information. Un cas d'utilisation courant des variables d'environnement est le partage de chemins, ainsi la variable JAVA_HOME est une variable habituellement utilisée pour définir le répertoire ou est installé Java.

Il existe une variable d'environnement spéciale PATH qui est présente par défaut. Comme son nom l'indique, elle contient les dossiers des exécutables accessibles en ligne de commande. En pratique, quand on saisi une commande (ls, java ...) en ligne de commande, le sysème recherche l'exécutable correspondant dans les dossiers listés dans la variable d'environnement PATH.

## Consulter les variables d'environnement disponibles dans la session courante

La commande **env** (ou son alter égo **printenv** ) permet d'afficher les noms et la valeur des variables d’environnement

{% highlight bash %}
env
{% endhighlight %}

## Consulter une variable d'environnement donnée
La commande suivante affiche la valeur de la variable **JAVA_HOME**

{% highlight bash %}
echo $JAVA_HOME
{% endhighlight %}

# Notion de scope variable d’environnement

Comme toute variable en informatique, les variables d’environnement ont un scope qui défini leur durée de vie. Dans un environneent Linux, on pourrait distinguer trois scopes:

* Le scope global
* Le scope utilisateur
* Le scope session

## Le scope global
Ce scope correspond au cas ou la variable doit être disponible pour tous les utilisateurs même si ces derniers pourront la surchager dans leur propre configuration.

## Le scope utilisateur
Il s'agit du cas ou un utilisateur donné souhaite définir des variables pour son propre besoin sans pour autant changer la configuration commune de la machine. Lorsqu'une variable est créée de cette manière, seul l'utilisateur concerné y a accès, les autres utilisateurs ne la verront pas

## Le scope session
Il s'agit d'un usage "à la volée", quand on a besoin d'une variable pour un traitement dans la session courante. Dans ce cas, on ne souhaite pas garder trace de cette variable au delà de la session courante. Ainsi, si  on ouvre un second terminal en plus du terminal courant, on peut constater qu'on a plus accès à la variable.


# Quelques précisions sur les fichiers lancés au "démarrage"
Définir une variable d'environnement va consister dans nombre de cas, à écrire une instruction dans un fichier exécutable lancé par le système. Il importe d'avoir une connaissance minimal des fichiers lancés au démarrage pour savoir dans lequel déclarer notre variable d'environnement.
*  */etc/profile, ~/.bash_profile, ~/.bash_login, ~/.profile* sont appelés dans cet ordre à chaque fois qu'un utilisateur se connecte
* *~/.bashrc* est appelé entre autres à chaque fois qu'un terminal est lancé

En pratique  *~/.bash_profile* est le lieu idéal pour définir la configuration propre à un utilisateur courant tandis que */etc/profile* sera adapté aux configurations devant impacter tous les utilisateurs.

# Création d'une variable d’environnement

La manière de créer une variable d'environnement va dépendre du scope choisi. Prenons l'exemple de la variable JAVA_HOME évoquée précédemment et imaginons que nous avons une machine avec plusieurs utilisateurs. Notre besoin est le suivant :
* Définir une variable JAVA_HOME par défaut valable pour tous les utilisateurs. cette variable va pointer vers l'install de Java 8 Update 10
* Pour l'utilisateur crepin_fadjo, nous souhaiterions surchager la variable JAVA_HOME afin qu'elle pointe sur  Java 8 Update 101
* Enfin pour la session courante de l'utilisateur crepin_fadjo, nous souhaiterions que la variable JAVA_HOME pointe vers l'install de Java 8 Update 121.

# Création d'une variable d’environnement pour la session courante
Il suffira de déclarer la variable directement en ligne de commande.

{% highlight bash %}
export JAVA_HOME=/opt/jdk1.8.0_121/
{% endhighlight %}
Si nous ouvrons une nouvelle session, nous pouvons constater que notre variable n'y existe pas.

# Création d'une variable d’environnement pour l'utilisateur courant

En étant connecté en tant que crepin_fadjo :

{% highlight bash %}
sudo echo "export JAVA_HOME=/opt/jdk1.8.0_101/" >> /home/crepin_fadjo/.bash_profile
{% endhighlight %}
Si nous changeons d'utilisateur, nous pouvons constater que notre variable n'y existe pas. Par contre, si crepin_fadjo se reconnecte, il verra à nouveau *JAVA_HOME*

# Création d'une variable d’environnement pour tous les utilisateurs
Il nous faut définir la variable dans un fichier lancé au démarrage quelque soit l'utilisateur qui se connecte. Comme indiqué précédemment, */etc/profile* est adapté pour ce besoin.

{% highlight bash %}
sudo echo "export JAVA_HOME=/opt/jdk1.8.0_10/" >> /etc/profile
{% endhighlight %}

Il faut noter que l'utilisateur crepin_fadjo continuera à avoir JAVA_HOME=/opt/jdk1.8.0_101 car il a surchagé la config réalisée dans /etc/profile

