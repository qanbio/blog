---
layout: post
title: Installer Java en ligne de commande sur une machine Linux
categories: [tech]
tags: [java, linux, debian, jdk, jre, installation, setup]
author: paterne_gaye
comments: true
fullview: true
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

Pour obtenir la valeur de l'URL, il faut se rendre sur le site d'Oracle.

![Page de téléchargement de Java](../../../../assets/media/2016-04-09-installation-java-ligne-de-commande-linux/java-identify-url.png "Page de téléchargement de Java")

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

![Téléchargement de Java](../../../../assets/media/2016-04-09-installation-java-ligne-de-commande-linux/java-wget-bon.png "Téléchargement de Java")


A la fin de cette étape, vous aurez un fichier **jdk-8u101-linux-x64.tar.gz** . Vérifier ceci en exécutant la commande :

{% highlight bash %}
ls -la
{% endhighlight %}

![Vérifier si l'archive de Java a été téléchargé](../../../../assets/media/2016-04-09-installation-java-ligne-de-commande-linux/java-ls-apres-wget.png "Vérifier si l'archive de Java a été téléchargé")


## Extraire les fichiers de l'archive
Nous allons extraire l'archive dans le dossier /opt  qui est historiquement réservé aux packages "optionnels", c'est à dire ne figurant pas dans les logiciels "par défaut" installés.

{% highlight bash %}
sudo tar zxvf jdk-8u101-linux-x64.tar.gz -C /opt
{% endhighlight %}

![Extraction de l'archive Java](../../../../assets/media/2016-04-09-installation-java-ligne-de-commande-linux/java-extract-archive.png "Extraction de l'archive Java ")


A l'issue de cette étape, vous devriez avoir un nouveau répertoire dans le dossier */opt* : **jdk1.8.0_101**

Vérifier ceci en exécutant la commande :
{% highlight bash %}
ls -la /opt/
{% endhighlight %}

![Vérification extraction de l'archive Java](../../../../assets/media/2016-04-09-installation-java-ligne-de-commande-linux/ls-la-opt-apres-extract.png "Vérification extraction de l'archive Java ")


A cette étape, java est installé. Vérifier ceci en exécutant la commande :

{% highlight bash %}
/opt/jdk1.8.0_101/bin/java -version
/opt/jdk1.8.0_101/bin/javac -version
{% endhighlight %}

![Vérification version de Java](../../../../assets/media/2016-04-09-installation-java-ligne-de-commande-linux/verif-version-java-javac.png "Vérification version de Java ")

# Variables d'environnement
*JAVA_HOME* est  utilisée par de nombreux logiciels pour déterminer l’emplacement ou est installé Java. Configurer cette variable est quasiment indispensable.

Il nous faut créer la [variable d'environnement](http://blog.qanbio.com/tech/2017/02/07/variables-environnement-linux.html "plus d'infos sur cet article") JAVA_HOME qui dans notre cas, cette variable aura la valeur */opt/jdk1.8.0_101/*

{% highlight bash %}
export JAVA_HOME=/opt/jdk1.8.0_101
{% endhighlight %}


Il faut ensuite rajouter le dossier JAVA_HOME dans la variable PATH.
{% highlight bash %}
export PATH=$JAVA_HOME/bin:$PATH
{% endhighlight %}

![Config var env](../../../../assets/media/2016-04-09-installation-java-ligne-de-commande-linux/java-conf-finale-avec-varenv.png "Config var env")


Maintenant pour exécuter java, nous n'avons plus besoin de préciser le chemin complet
{% highlight bash %}
java -version
javac -version
{% endhighlight %}
