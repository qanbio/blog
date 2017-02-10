---
layout: post
title: Installer Java en ligne de commande sur une machine Linux
categories: [tech]
tags: [java, linux, debian, jdk, jre, installation, setup]
author: paterne_gaye
comments: true
fullview: false
description: Ce post est un aide-mémoire pour installation en ligne de commande à partir de fichiers .tar.
---

# Pré-requis

## JDK ou JRE ?

Installer Java pour développer sous-entend deux aspects :

* installer la JDK qui fournit toutes les classes dont vous aurez besoin
* installer la JRE qui va exécuter votre code Java

La JRE est inclue dans la JDK donc nous allons télécharger directement la JDK.


## Identifier les URLs de téléchargement

Il y a trois paramètres :

* Le numéro de version, qui indique la version de Java, 5, 6, 7 ou 8 par exemple et le numéro d'update dans la version, 80 ou 101 par exemple
* Le système d'exploitation : Linux, Windows etc.
* L'architecture X86, 64 bits, 32 bits

Dans ce qui suit nous installerons la JDK 8, update 101, pour une machine Linux 64 bits.

Pour obtenir la valeur de l'URL, il faut se rendre sur le site d'Oracle. Au moment de la rédaction de cet artoicle, la page en question est [ici](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html "")

A noter que tant que vous n'aurez pas cocher le bouton radio "Accept License Agreement", le lien ne sera pas accessible.

Une fois le lien de votre version ainsi obtenu copiez le quelque part et connectez-vous à votre machine linux pour la suite.


# Installation

Avant tout positionnez vous dans votre dossier *home* en exécutant la commande suivante

{% highlight shell %}
cd
{% endhighlight %}


## Téléchargement de l'archive

{% highlight bash %}
wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie"  http://download.oracle.com/otn-pub/java/jdk/8u101-b13/jdk-8u101-linux-x64.tar.gz
{% endhighlight %}

A la fin de cette étape, vous aurez un fichier **jdk-8u101-linux-x64.tar.gz** . Vérifier ceci en exécutant la commande :

{% highlight bash %}
ls -la
{% endhighlight %}


## Extraire les fichiers de l'archive
Nous allons extraire l'archive dans le dossier /opt  qui est historiquement réservé aux packages "optionnels", c'est à dire ne figurant pas dans les logiciels "par défaut" installés.

{% highlight bash %}
sudo tar zxvf jdk-8u101-linux-x64.tar.gz -C /opt
{% endhighlight %}

A l'issue de cette étape, vous devriez avoir un nouveau répertoire dans le dossier */opt* : **jdk1.8.0_101**

Vérifier ceci en exécutant la commande :
{% highlight bash %}
ls -la
{% endhighlight %}

A cette étape, java est installé. Vérifier ceci en exécutant la commande :

{% highlight bash %}
/opt/jdk1.8.0_101/bin/java -version
{% endhighlight %}


# Configuration

#### Créer la [variable d'environnement](http://blog.qanbio.com/tech/2017/02/07/variables-environnement-linux.html "plus d'infos sur cet article") JAVA_HOME

### A quoi sert JAVA_HOME?

JAVA_HOME est  utilisée par de nombreux logiciels pour déterminer l’emplacement ou est installé Java. Configurer cette variable est quasiment indispensable.

Pour créer donc JAVA_HOME il faut exécuter la commande

{% highlight bash %}
export JAVA_HOME=/opt/jdk1.8.0_101
{% endhighlight %}

#### Rajouter JAVA_HOME/bin au PATH

{% highlight bash %}
export PATH=$JAVA_HOME/bin:$PATH
{% endhighlight %}
