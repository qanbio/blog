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
Avoir une certaine connaissance de l’IDE (Envionnement de développement Intégré) [Android Studio](https://developer.android.com/studio/index.html). Intégrer à votre projet, une API de localisation fournie par Google Play Services (G.P.S.) , en ajoutant ce qui suit au fichier app.gradle : {% highlight bash %}compile 'com.google.android.gms:play-services-location:11.0.2'{% endhighlight%}. Intégrer un gestionnaire de permissions à la volée pour Android M et plus, en ajoutant ce qui suit au fichier app.gradle : {% highlight bash %}compile 'com.karumi:dexter:4.1.0'{% endhighlight%}. Le code source relatif à billet est disponible [ici]().

### Connaitre la position actuelle de l'utilisateur
Pour connaitre la position actuelle de l'utilisateur, votre application devra identifier la **récente position géographique connue** (the Last Known Location) occupée par le périphérique, sur le globe terrestre. Identifier cette position pour un appareil android, équivaut à reconnaitre __a priori__ la position actuelle de son utilisateur. Votre application fera donc appel au fournisseur de localisation fusionnée (__Fused Location Provider__) de G.P.S.

#### Spécifier les permissions pour l'application
Pour commencer, il faut d'abord réclamer la permission d'accès aux positions géographiques de l'appreil, pour le compte de votre application. Faites comme suit,dans le fichier Manifest.xml :

![Réclamer la permission de localisation](../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/add_location_permissions_to_manifest.PNG "Réclamer la permission de localisation")

La permission ACCESS_COARSE_LOCATION permet d'obtenir une précision géolocalisée à l'échelle des villes. Vous pouvez aussi utiliser ACCESS_FINE_LOCATION. Si le système du périphérique est Android 6.0 ou supérieur, et que le SDK cible de votre application est égal ou supérieur à 23, alors votre application doit répertorier les autorisations dans le manifeste et demander ces autorisations au moment de l'exécution (voir [code source] (https://)).


#### Créer un client de localisation
La prochaine étape consiste à créer une instance de type **FusedLocationProviderClient**, dans la méthode onCreate() de votre activité. Imitez ce qui suit :

![Créer un client de localisation](../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/create__fuse_location_provider_instance.PNG "Créer un client de localisation")
 

#### Obtenir la récente position de l'appareil d'un utilisateur
Une fois le client de localisation créé, il suffit d'appeler la méthode __getLastLocation()__, pour déterminer la position actuelle de l'utlisateur. A cette méthode est rattachée une taupe, qui se déclenche lorsque la position est déterminée avec succès. En cas de succès, un objet de type Location est retourné, à partir duquel vous pourrez récupérer les coordonnées de latitude et de longitude de  l'emplacement géographique actuel de l'utilisateur.

![Obtenir la position actuelle](../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/call_get_last_location.PNG "Obtenir la position actuelle")
 

### Ajuster les paramètres de localisation
Dans la majorité des cas, une application de geolocalisation, invitera l'utilisateur à adjuster les paramètres de GPS ou Wi-Fi de son appareil, si nécessaire. C'est une démarche recommandée pour éviter des situations de crash, pour votre application.  Pour effectuer une telle demande, il faut :
* Créer une requete de localisation
* Récupérer et vérifier la configuration actuelle de l'appareil
* Inviter l'utilisateur à modifier la configuration de son appareil

#### Créer une requête de localisation
Créer une instance de type **LocalRequest** avec les contraintes telles que le niveau de précision (__setPriority()__), les intervalles en millisecondes pour le rafraichissement de la position (__setInterval()__ et __setFasterIntervall()__) :

![Créer une requete de localisation](../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/setup_location_request.PNG "Créer une requete de localisation")

#### Récupérer et vérifier la configuration actuelle
Pour récupérer, les détails de la configuration actuelle du prériphérique d'un utilisateur, il faut instancier la classe **LocationSettingsRequest.Builder** et lui ajouter un ou plusieurs objets LocalRequest. Ensuite, on vérifie si la configuration retrouvée sur l'appareil de l'utilisateur sont convenables pour les besoins de l'application :

![Récupérer et vérifier la configuration actuelle](../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/recuperer_et_verifier_configuration.PNG "Récupérer et vérifier la configuration actuelle")


### Conclusion 
En cours de rédaction