---
layout: post
title: Interagir avec d'autres applications avec les objets Intents
categories: [tech]
tags: [Android, Google Search, Deep link, Mobile]
author: fabrice_kiki
comments: true
fullview: false
description: Ce post montre comment faire interagir votre application avec les autres applications disponibles sur le téléphone de l'utilisateur.
---
# Prérequis
<a href="https://developer.android.com/studio/index.html?gclid=Cj0KCQjw95vPBRDVARIsAKvPd3Ljl-9BwN6bjz3QhsKkLdxyCAg9wpZzgkSsKCPhK7JfBBKxqWP6c7waAiQTEALw_wcB">Android Studio 2.3.3</a> ou une version supérieure.


# Problématique
Utiliser les [Intents](https://developer.android.com/reference/android/content/Intent.html) (intentions) pour permettre à votre application mobile d'interagir avec les autres applications et d'échanger des ressources avec elles.

# Concepts d’Intentions et d’activités

## Les Activités
 
Une [activité](https://developer.android.com/reference/android/app/Activity.html) représente un seul écran dans votre application Android et est liée à une interface avec laquelle l'utilisateur peut interagir. Par exemple, une application de messagerie peut avoir une activité qui affiche une liste de nouveaux messages, une autre activité pour composer un texto et une autre activité pour lire des messages archivés. Votre application est une collection d'activités que vous créez vous-même ou bien qui appartiennent aux autres applications, mais que vous réutilisez quand même.

<div align="center">
<img src="../../../../assets/media/2017-11-06-interacting-with-other-apps/apps-interactions/apps-interactions.png" alt="apps-interactions" height="534" width="300" ALIGN="middle">
</div>

Petit rappel pour implémenter une nouvelle activité à votre application dans Android Studio :

* Créer une instance JAVA de la classe  Activity ou AppCompatActivity

* Implémenter une interface utilisateur pour l’activité

* Déclarer l’activité dans le fichier manifest.xml  de l’application

## Les [Intents](https://developer.android.com/reference/android/content/Intent.html) (ou intentions)

Toute activité en Android est démarrée à partir d’une intention (intent). Les intentions sont des indications que les applications envoient au système du téléphone pour réclamer le démarrage d’une activité ou de n’importe quel autre composant. Votre application ne démarre pas ses activités, c’est plutôt le système Android qui fait  l’arbitrage de la gestion des activités.

Il y a 2 types d’intentions en Android :

* Les **Intents explicites**, vous permettent de cibler avec précision les activités déclarées dans votre application, histoire  de naviguer à travers les écrans de votre application)

* Les **Intents implicites** ne ciblent aucune activité en particulier. Vous déclarez l’action à exécuter à travers l’Intention, et le système du téléphone se charge de scanner les applications appropriées capabes d'exécuter le type d’action spécifiée.

# Démarrer une activité spécifique par une intention

Les intents sont des instances de la classe Intent.

Pour démarrer une activité spécifique, utilisez une intention explicite et la méthode startActivity (). Pour les intentions explicites, il faut obligatoirement préciser le nom de paquet de l'activité ciblée.. Tous les autres champs d'intention sont facultatifs, et sont mis à null par défaut.

Par exemple, si vous souhaitez démarrer une activité  nommée (c'est juste un nom pris au hasard) *AfficherMessageActivity* pour afficher un message spécifique dans une application de messagerie, vous pouvez utiliser le code suivant :

{% highlight bash %}
Intent messageIntent = new Intent(this, AfficherMessageActivity.class);
startActivity(messageIntent);
{% endhighlight%}

Le constructeur **Intent()** prend deux arguments pour une intention explicite :

* Le *contexte* pour appliquer l’intent (ici, c’est le *this*)

* La *cible* - ici, il s’agit de l’activité AfficherMessageActivity.

La méthode **startActivity()**  prend en argument l’objet *messageIntent* et l’envoie au système Android. Android excécute une instance AfficherMessageActivity, pour votre application. La nouvelle activité apparaît sur l'écran et l'activité appelante est mise en pause.