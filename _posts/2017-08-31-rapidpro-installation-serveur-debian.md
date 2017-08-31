---
layout: post
title: Mise en place d'un serveur RapidPro sur un serveur debian
categories: [tech]
tags: [linux, debian, rapidpro]
author: 
comments: true
fullview: false
description: Ce post est un aide-mémoire pour la mise en place du serveur rapidpro sur un serveur debian 
---

# Qu'est ce que RapidPro?
RapidPro est une plateforme de gestion de flux d’information, en temps réel, proposée par l’UNICEF. La plateforme a été développée en 2014, en collaboration avec la société rwandaise de logiciels Nyaruka.


# Mise en place de RapidPro
La mise en place d’un serveur RapidPro suppose le déploiement d’une application Web complexe intégrant des bases de données PostgreSQL et Redis ainsi que plusieurs outils de développement côté serveur tels que NodeJS. Dans ce qui va suivre, nous allons successivement installer ces différents composants, y compris l’environnement de développement Python. 

Créer un utilisateur temba avec les droits d’administrateur
Installation de NodeJS et ses modules
Installation de Redis 
Installation de Less
Installation de CoffeeScript
Installation de libmagic-dev
Installation de PostgreSQL
Récupérer le code source de RapidPro
Construire l’environnement virtuel

