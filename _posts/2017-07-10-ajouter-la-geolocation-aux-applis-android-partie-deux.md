---
layout: post
title: Ajouter la géolocation à votre application Android (2/2)
categories: [tech]
tags: [android, java, géolocalisation, location API, android studio, application mobile, google, développement]
author: fabrice_kiki
comments: true
fullview: false
description: Intégrer la géolocalisation à votre application mobile Android, pour proposant aux utilisateurs des informations et services, relatifs à leur position géographique et à leur environnement.
---
### Problématique :
Renouveller automatiquement les informations sur la position de l'utilisateur, en temps réel.

### Pré requis
Avoir une certaine connaissance de l’IDE (Envionnement de développement Intégré) [Android Studio](https://developer.android.com/studio/index.html). Intégrer à votre projet, une API de localisation fournie par Google Play Services (G.P.S.) , en ajoutant ce qui suit au fichier app.gradle : {% highlight bash %}compile 'com.google.android.gms:play-services-location:11.0.2'{% endhighlight%}. Intégrer un gestionnaire de permissions à la volée pour Android M et plus, en ajoutant ce qui suit au fichier app.gradle : {% highlight bash %}compile 'com.karumi:dexter:4.1.0'{% endhighlight%}. Le code source comptet opérationnel sera affiché à la fin du tutoriel.

### Corps de l'article
 Dans le [précédent billet] (<http://blog.qanbio.com/tech/2017/07/10/ajouter-la-geolocation-aux-applis-android.html>), une application utilise Google Play Services pour identifier la position actuelle d'un utilisateur. Le présent article présente la métholodie qui permet à une application android de recevoir des mise-à-jour réguliers sur l'endroit où se trouve un utilisateur.

### Conclusion 
La plateforme Android, vous permet de différencier votre application, en utiliseant la puissance de la mobilité pour donner à vos utilisateurs des informations contextuelles sur l'endroit où ils se trouvent, quand ils s'y trouvent. À vous de faire vivre à vos utilisateurs, des expériences inoubliables avec votre application. Ils vous remercieront. Bonne chance !!
