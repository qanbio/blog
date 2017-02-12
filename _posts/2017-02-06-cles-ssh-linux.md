---
layout: post
title: Génération de clés SSH sous Linux
categories: [tech]
tags: [linux, debian, ssh]
author: crepin_fadjo
comments: true
fullview: false
description: Ce post est un aide-mémoire pour la génération de clés SSH sous linux.
---

# Notion de clés SSH

>Secure Shell (SSH) est à la fois un programme informatique et un protocole de communication sécurisé. Le protocole de connexion impose un échange de clés de chiffrement en début de connexion. Par la suite, tous les segments TCP sont authentifiés et chiffrés. Il devient donc impossible d'utiliser un sniffer pour voir ce que fait l'utilisateur.

>Le protocole SSH a été conçu avec l'objectif de remplacer les différents programmes rlogin, telnet, rcp, ftp et rsh.

Source:wikipédia

# Usage des clés SSH
Les clés SSH sont un des moyens d'utiliser le protocole SSH.
Les clé ssh s’utilisent par paire : une publique et une privée; la publique servant à crypter le message el la privée à la décrypter.
Pour un developpeur, elles ont un intéret pratique évident : ne pas avoir à saisir ses identifiants (login/password) à chaque connection sur une machine tiers.
Par exemple, pour se connecter à Github, une fois, notre clé publique connue de Github, nous avons juste à crypter nos messages à l'aide de notre clés privée et nous pourrons nous connecter à Github sans avoir à fournir de username:password


# Génération d'une paire de clés ssh

Nous pouvons générer une paire de clés SSH à l'aide de la commande suivante

{% highlight bash %}
ssh-keygen -t rsa -b 4096 -C "Jenkins"
{% endhighlight %}

* -t: Indique l'agorithme utilisé pour générer des clés, en l'occurrence RSA

* -b: Indique la longueur de la clé en bits, ici 4096

* -C: Permet de rajouter un commentaire. Par exemple, nous indiquons en commentaire "Jenkins" car nous comptons utiliser cette clé pour notre instance Jenkins.

A l'issue de cette commande, nous avons dans le dossier *~/.ssh* deux fichiers :
* id_rsa ou est stockée la clé privée
* id_rsa.pub ou est stockée la clée publique

