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
Jenkins est un outil open source [d'intégration continue](https://fr.wikipedia.org/wiki/Int%C3%A9gration_continue "lien vers wikipedia") développé en Java.

# Installation de Jenkins
Jenkins est une application web Java et pour l'installation, il est possible de la déployer sur un container de servlet tel que Apache Tomcat. Mais il est également possible d'installer Jenkins de manière autonome sous forme de service Linux. C'est cette dernière solution que nous avons adoptée car nous paraissant plus simple et efficace.

## Configurer la communication entre notre machine Linux et le repo de Jenkins
{% highlight bash %}
 wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
{% endhighlight %}

![image](../../../../assets/media/2017-02-10-jenkins-installation-linux/jenkins-add-key.png " ")


## Ajouter le repo de Jenkins dans la liste des repo de notre machine Linux
{% highlight bash %}
sudo vi /etc/default/jenkins
{% endhighlight %}

![image](../../../../assets/media/2017-02-10-jenkins-installation-linux/vi-etc-apt-sources-list.png " ")

## Mettre à jour le système et installer le "paquet jenkins"

{% highlight bash %}
sudo apt-get update
sudo apt-get install jenkins
{% endhighlight %}

![image](../../../../assets/media/2017-02-10-jenkins-installation-linux/install-jenkins.png " ")


## Demarrer Jenkins
Nous venons d'installer Jenkins comme un service Linux; Ceci peut se vérifier en consultant le contenu du dossier /etc/init.d/
{% highlight bash %}
 ls -la /etc/init.d/
{% endhighlight %}

![image](../../../../assets/media/2017-02-10-jenkins-installation-linux/ls-la-jenkins-service.png " ")

Nous pouvons désormais démarrer et stoper jenkins  comme un service

Cependant, il peut être intéressant d'effectuer de modifier d'abord le port de Jenkins. En effet, Jenkins démarre par défaut sur le port 8080 or ce port est celui part défaut du serveur Apache. La modification de port permet d'éviter des dysfonctionnement dus a une éventuelle collusion de ports.

Pour ce faire, il faut ouvrir le fichier /etc/default/jenkins avec la commande :
{% highlight bash %}
sudo vi /etc/default/jenkins
{% endhighlight %}

![image](../../../../assets/media/2017-02-10-jenkins-installation-linux/jenkins-change-port.png " ")

Ce paramétrage effectué, nous pouvons démarrer notre service Jenkins.

{% highlight bash %}
sudo /etc/init.d/jenkins start
{% endhighlight %}

![image](../../../../assets/media/2017-02-10-jenkins-installation-linux/start-jenkins.png " ")

La webapp Jenkins est ainsi démarré en HTTP sur le port 3333.


## Configuration

### Débloquer Jenkins
Jenkins est certes démarré mais un petit test en ligne de commande nous montre que nous avons un problème de droits

![image](../../../../assets/media/2017-02-10-jenkins-installation-linux/jenkins-wget-avec-echec-403.png" ")

Ceci est confirmé par un accès à la page d'accueil dans le navigateur  (HTTP sur port 3333 dans notre cas).

![image](../../../../assets/media/2017-02-10-jenkins-installation-linux/jenkins-accueil-avant-passwd.png " ")

En suivant les instruction et copiant le mot de passe administrateur depuis le fichier indiqué, nous "débloquons Jenkins"

A cette étape il faut se connecter avec le mode de passe par défaut envoyé par jenkins

### Installations des plugins
Nous sommes ensuite invités à installer les plugins. Pour commencer nous pouvons prendre les plugins usuels recommandés par la communauté.

![image](../../../../assets/media/2017-02-10-jenkins-installation-linux/Jenkins-choix-plugins.png " ")


## Ajout des comptes utilisateurs
Une fois, l'étape des plugins passée, nous pouvons créer notre premier utilisateur :

![image](../../../../assets/media/2017-02-10-jenkins-installation-linux/jenkins-creation-first-user.png " ")

Cette étape conclu l'installation

![image](../../../../assets/media/2017-02-10-jenkins-installation-linux/jenkins-install-terminee.png " ")

