---
layout: post
title: Laisser Google Search indexer une application Android
categories: [tech]
tags: [Android, Google Search, Deep link, Mobile]
author: fabrice_kiki
comments: true
fullview: false
description: Ce post montre comment ajouter des Deep link à une application Mobile avec Android Studio.
---
# Prérequis

<a href="https://developer.android.com/studio/index.html?gclid=Cj0KCQjw95vPBRDVARIsAKvPd3Ljl-9BwN6bjz3QhsKkLdxyCAg9wpZzgkSsKCPhK7JfBBKxqWP6c7waAiQTEALw_wcB">Android Studio 2.3.3</a> ou une version supérieure.

# Problématique

Permettre à Google Search de présenter votre application Android comme destination, lorsquLe l'utilisatieur clique sur un lien ou un résult de recherche, dont le contenu correspond à votre site web.

# Solution

Ajouter des **App Links** Android à votre application en suivant les étapes suivantes :

* Définissant des Intent Filters qui ouvrent le contenu précis de votre application à l'aide des URL HTTP (ou Deep Link).
* Prouver que vous êtes effectivement le propriétaire, à la fois votre application et les URL du site Web.

Dans le présent article, nous allons nous focaliser sur comment ajouter les Intent Filters, et comment définir les Deep Links avec Android Studio.

# Concept de App Link et Deep Link

Avant d'aller plus loin dans le post, clarifions les concoepts de **Deep links** et **App Links** pour Android.

## Deep Links (Liens profonds)

Ce sont des URL, qui amènent  l'utilisateur directement vers un contenu spécifique dans votre application (ex: affichage d'un text, d'une image, d'une vidéo, etc...). En Android, il est possible de définir des Deep Links en ajoutant des <a href="https://developer.android.com/guide/components/intents-filters.html">Intents Fiters</a> puis, d'extraire des informatiosn utiles des Intents appelant votre application, pour déterminer l'activité convemable à lancer.

## Android App Links

Introduits à partir de la version Marshmallow de l'OS Android, les App Links permettent à une application d'être désignée par l'utilisateur comme étant l'application par défaut à lancer pour ouvrir un lien sur le téléphone. Et si  l'utilisateur ne désire plus utiliser une application comme application par défaut, il peut toutefois la révoquer dans les paramètres de son téléphone. Ajouter des App Links à une application présente plusieurs avantages :

* Les liens que vous indexez dans votre application sont ceux de votre site web
* Les utilisateurs qui n'ont pas accès à votre site web, peuvent toutefois avoir le contenu recherché sur leur téléphone
* Les utilisateurs sont plus engagés parce qu'ils accècent directement au contenu de votre application web, depuis le moteur de recherche Google. Ils n'ont pas besoin de passer par un protocole  connexion pour les contenus généraux et publics.

# Créer des Intent Filter pour les URIs



# Gérer les appels de votre Deep Link


# Tester votre Deep Link


# Conclusion
