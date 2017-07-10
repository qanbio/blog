---
layout: post
title: Ajouter la géolocation à votre application Android
categories: [tech]
tags: [android, java, géolocalisation, location API, android studio, application mobile, google, développement]
author: fabrice_kiki
comments: true
fullview: false
description: Intégrer la géolocalisation à votre application mobile Android, pour apporter aux utilisateurs une expérience plus authentique et réaliste.
---
### Problématique :
Rendre votre application Android, plus utile et intuitive, en proposant aux utilisateurs des informations et services, relatifs à leur position géographique et à tout ce qui se passe autour d'eux.

### Pré requis
Avoir une certaine connaissance de l’IDE (Envionnement de développement Intégré) [Android Studio](https://developer.android.com/studio/index.html). Intégrer à votre projet, une API de localisation fournie par Google Play Services (G.P.S.) (en ajoutant ce qui suit au fichier app.gradle : {% highlight bash %}compile 'com.google.android.gms:play-services-location:11.0.2'{% endhighlight%}). Intégrer un gestionnaire de permissions à la volée pour Android M et plus (en ajoutant ce qui suit au fichier app.gradle : {% highlight bash %}compile 'com.karumi:dexter:4.1.0''{% endhighlight%})

### Connaitre la position actuelle de l'utilisateur
Pour connaitre la position actuelle de l'utilisateur, votre application devra identifier la **récente position géographique connue** (the Last Known Location)par l'appareil de celui-ci sur le globe terrestre. Identifier cette position pour un appareil android, équivaut à reconnaitre __a priori__ la position actuelle de son utilisateur. Votre application fera donc appel au fournisseur de localisation fusionnée (__Fused Location Provider__) de G.P.S.

## Spécifier les permissions pour l'application
Pour commencer, il faut d'abord réclamer la permission d'accès aux positions géographiques de l'appreil, pour le compte de votre application. Faites comme suit,dans le fichier Manifest.xml :

![Réclamer la permission de localisation](../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/add_location_permissions_to_manifest.PNG "Réclamer la permission de localisation")

La permission ACCESS_COARSE_LOCATION permet d'obtenir une précision géolocalisée à l'échelle des villes. Vous pouvez aussi utiliser ACCESS_FINE_LOCATION


## Créer un client de localisation
La prochaine étape consiste à créer une instance de type **FusedLocationProviderClient**, dans la méthode onCreate() de votre activité. Imitez ce qui suit :
![Créer un client de localisation](../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/create__fuse_location_provider_instance.PNG "Créer un client de localisation")
 

## Obtenir la récente position de l'appareil d'un utilisateur
Une fois le client de localisation créé, il suffit d'appeler la méthode __getLastLocation()__, pour déterminer la position actuelle de l'utlisateur. A cette méthode est rattachée une taupe, qui se déclenche lorsque la position est déterminée avec succès. En cas de succès, un objet de type Location est retourné, à partir duquel vous pourrez récupérer les coordonnées de latitude et de longitude de  l'emplacement géographique actuel de l'utilisateur.
![Obtenir la position actuelle](../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/call_get_last_location.PNG "Obtenir la position actuelle")
 


### Inviter l'utiliser à ajuster ses paramètres de localisation
 En cours de rédaction


### Conclusion 
En cours de rédaction
