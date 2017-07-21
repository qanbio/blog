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

### Démaarer l'actualisation de la position 
 Dans le [précédent billet] (<http://blog.qanbio.com/tech/2017/07/10/ajouter-la-geolocation-aux-applis-android.html>), une application utilise Google Play Services pour identifier la position actuelle d'un utilisateur, obtenue en invoquant la méthode  **getLastLocation()** sur un client de localisation de type **FusedLocationProviderClient**.

 Par ailleurs, actulaiser la position géolocalisée de l'utilisateur est particulièrement utile, lorsque l'application offre un service de suivi de livraison, ou un service d'aide à l'orientation en zone urbaine. Pour ce faire, il faut :
  * appliquer la méthode  **requestLocationsUpdates()** sur une l'instance de FusedLocationProviderClient.
  * ajouter en paramètres, une ou plusieurs positions (objets **Location**) préalablement identifiées
  * rattacher une taupe de type **LocationCallback** pour récupérer les positions actualisées
  
  Voici comment cela se présente dans une méthode que nous avons nommé __startLocationUpdates__ :
  ![actualiser les positions de l'utilisateur]("../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/start_location_update.PNG")

### Implémenter une taupe de type LocationCallback

![implementer la taupe] ("../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/implement_location_callback.PNG")

### Arrêter les mises-à-jour de localisation
Lorsque l'application passe en pause sur l'appareil de l'utilisateur, pour une raison pou une autre, il est recommandé de désactiver la mise-à-jour de la localisation. 

![implementer la taupe] ("../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/stop_location_updates.PNG")


### Conclusion 
La plateforme Android, vous permet de différencier votre application, en utiliseant la puissance de la mobilité pour donner à vos utilisateurs des informations contextuelles sur l'endroit où ils se trouvent, quand ils s'y trouvent. À vous de faire vivre à vos utilisateurs, des expériences inoubliables avec votre application. Ils vous remercieront. Bonne chance !!