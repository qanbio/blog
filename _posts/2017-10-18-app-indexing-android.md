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
 

# Créer des Intent Filter pour les URIs


# Gérer les appels de votre Deep Link


# Tester votre Deep Link


# Conclusion
