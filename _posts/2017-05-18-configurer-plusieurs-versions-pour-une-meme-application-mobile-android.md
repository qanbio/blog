---
layout: post
title: Configurer-plusieurs-versions-pour-une-meme-application-mobile-android
categories: [tech]
tags: [android, java, flavors, gradle, android studio, application mobile, google, versionning, build type, apk, app variants]
author: fabrice_kiki
comments: true
fullview: false
description: Ce post est un aide-mémoire pour gérer le déploiement de plusieurs versions d'une même application Android avec Android Studio.
---
### Problématique :
Développer une application Android devant être déployée en plusieurs versions (fichiers .APK) dans différentes cibles , notamment : **dev** (pour le développement), **recette** (pour la recette), **prod** (pour la version stable et opérationnelle de l’application), avec des noms, des logos, des fonctionnalités, ou autres variantes significatives.

### Pré requis
Avoir une certaine connaissance de l’IDE (Outil de développement Intégré) [Android Studio](https://developer.android.com/studio/index.html).

### Solutions possibles
Une solution d’amateur (et coûteuse !) consisterait à avoir une version originale du projet avec une multitude de duplicatas pour chaque environnement et y intégrer des modifications en parallèle. Cette méthode est bien entendue pas du tout flexible et peut à la longue induire des complications dans la maintenance du code, au fur et à mesure que le projet prendra de 
l’ampleur !
Une seconde solution serait quand même dans un premier temps d’élaborer une version stable avec des fonctionnalités minimales requises du projet. Puis, de créer un nouveau projet d’application pour chaque environnement puis d’y intégrer le projet stable sous forme de librairie. Cela pourrait bien fonctionner, puisque tout le code ne sera pas répliqué. Néanmoins, au final on aura créé une application-librairie mais pas une application autonome.

Cependant, ’il existe une autre approche plus simple et plus flexible. Une approche avec laquelle n’importe quel développeur et son équipe peuvent gérer et déployer plusieurs versions d’une application Android avec une seule copie du projet, comme sus-cité. Cette technique est déjà intégrée dans l’IDE officiel d’Android Studio et repose sur son moteur de compilation Gradle. C’est la méthode des flavor (c-à-d “saveur” en Anglais). Les flavor permettent d’avoir une multitude de variantes d’une même application Mobile avec chacune ses éléments de différenciation, tout en assurant le maintien des points communs qui font l’originalité du projet. 

### Comment marche les Flavors

Android Studio génère les variantes d’application (ou Build Variant) en combinant  chaque Flavor de l’application avec un “type de compilation” (ou Build Type).
Un type de compilation spécifie les critères de compilation et de mise en paquet (fichier APK) d’une application tels que la clé de chiffrement, l’activation du débogage, ainsi que la minification de la taille du fichier APK.  En complément, un Flavor spécifie les critères tels que les ressources à exploiter (layouts, code java, images), les versions OS compatibles, ou  les dimensions d’écran autorisées. Par défaut, Android Studio spécifie automatiquement deux types de compilation : debug (débogage) et release (production), pour chaque projet d’application.

Par exemple, En combinant deux types de build “debug”  et “release” avec les flavors “dev”, “recette” et “prod”, on obtiendra les variantes suivantes pour une même application :

⋅⋅*devDebug
⋅⋅*devRelease
⋅⋅*recetteDebug
⋅⋅*recetteRelease
⋅⋅*prodDebug
⋅⋅*prodRelease

Nous allons créer une simple application avec les trois flavor sus-indiqués, et les exploiter pour personnaliser les différentes versions du projet pour chaque environnement correspondant.
 

