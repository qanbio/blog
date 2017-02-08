---
layout: post
title: Les variables d'environnement sur une machine Linux
categories: [tech]
tags: [linux, debian, env, var, variable, environnement]
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

Les clé ssh s’utilisent par paire : une publique et une privée; la publique servant à crypter le message el la privée à la décrypter.En pratique les clés ssh sont généré de plusieurs manières.Dans notre cas on va la générer avec ssh-keygen

# Génération d'une paire de clés ssh

Nous pouvons générer une paire de clés SSH à l'aide de la commande suivante

{% highlight bash %}
ssh-keygen -t rsa -b 4096 -C "Jenkins"
{% endhighlight %}

* -t: Indique l'agorithme utilisé pour générer des clés, en l'occurrence RSA

* -b: Indique la longueur de la clé en bits, ici 4096

* -C: Permet de rajouter un commentaire. Par exemple, nous indiquons en commentaire "Jenkins"

A l'issue de cette commande, nous avons dans le dossier *~/.ssh* deux fichiers :
* id_rsa ou est stockée la clé privée
* id_rsa.pub ou est stockée la clée publique

