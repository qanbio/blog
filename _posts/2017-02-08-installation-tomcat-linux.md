---
layout: post
title: Installer Tomcat en ligne de commande sur une machine Linux
categories: [tech]
tags: [java, linux, debian, tomcat, installation, setup,apache]
author: crepin_fadjo
comments: true
fullview: false
description: Ce post est un aide-mémoire pour l'installation en ligne de commande de tomcat à partir de fichiers .tar.
---

## Identifier les URLs de téléchargement

Il y a trois paramètres :

* Le numéro de version, qui indique la version de Tomcat 6, 7, 8, 9 par exemple et le numéro d'update dans la version, 5.11 ou 0.41 par exemple
* Le système d'exploitation : Linux, Windows etc.
* L'architecture X86, 64 bits, 32 bits

Dans ce qui suit nous installerons la Tomcat 8, update 5.11, pour une machine Linux 64 bits.

Pour obtenir la valeur de l'URL, il faut se rendre sur le site d'Oracle. Au moment de la rédaction de cet article, la page en question est [ici](http://tomcat.apache.org/download-80.cgi"")

# Installation

Avant tout positionnez vous dans votre dossier *home* en exécutant la commande suivante

{% highlight shell %}
cd
{% endhighlight %}


## Téléchargement de l'archive

{% highlight bash %}
wget  http://www-us.apache.org/dist/tomcat/tomcat-8/v8.5.11/bin/apache-tomcat-8.5.11.tar.gz
{% endhighlight %}

A la fin de cette étape, vous aurez un fichier **apache-tomcat-8.5.11.tar.gz** . Vérifier ceci en exécutant la commande :

{% highlight bash %}
ls -la
{% endhighlight %}


## Extraire les fichiers de l'archive
Nous allons extraire l'archive dans le dossier /opt  qui est historiquement réservé aux packages "optionnels", c'est à dire ne figurant pas dans les logiciels "par défaut" installés.

{% highlight bash %}
sudo tar zxvf apache-tomcat-8.5.11.tar.gz -C /opt
{% endhighlight %}

A l'issue de cette étape, vous devriez avoir un nouveau répertoire dans le dossier */opt* : **apache-tomcat-8.5.11**

Vérifier ceci en exécutant la commande :
{% highlight bash %}
ls -la
{% endhighlight %}

Tomcat démarre sur le port 8080.Vous pouez démarrer Tomcat en exécutant la commande

Vérifier ceci en exécutant la commande :

{% highlight bash %}
sudo /opt/apache-tomcat-8.5.11/bin/startup.sh
{% endhighlight %}

On peut maintenant accéder à Tomcat via un navigateur web en faisant :

{% highlight bash %}
<Addresse IP>:<Port>
{% endhighlight %}