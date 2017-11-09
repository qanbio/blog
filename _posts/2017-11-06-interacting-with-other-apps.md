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
Utiliser les [Intents](https://developer.android.com/reference/android/content/Intent.html) (intentions) pour permettre à votre application mobile d'interagir avec les autres applications et d'échanger des ressources avec celles-ci.

# Concepts d’Intentions et d’activités

Une application Android est avant tout une collection d'[activités](https://developer.android.com/reference/android/app/Activity.html). Chaque activité représente un écran de cette application. Pour permettre à l’utilisateur de naviguer d'un écran à un autre, les applications utilisent le mécanisme  des Intent (ce qui signifie littéralement intention). En Android, la classe [Intent](https://developer.android.com/reference/android/content/Intent.html) permet à une application de fournir au téléphone, des paramètres utiles pour déclencher l’affichage d’une activité, dans le but d’effectuer des tâches précises (par exemple, démarrer une activité pour prendre une photo, une autre pour envoyer un email, et consorts).

<br/>
<div align="center">
<img src="../../../../assets/media/2017-11-06-interacting-with-other-apps/apps-interactions.png" alt="apps-interactions" height="428" width="300" ALIGN="middle">
</div>
<br/>

# Les [Intents](https://developer.android.com/reference/android/content/Intent.html) (ou intentions)

Il y a 2 types d’intentions en Android :

## Intents explicites

Les *Intents* explicites sont utilsés dans les cas où l'on veut démarrer une activité dont on connait précisément le nom (et idéalement le nom de package). Il suffit de préciser ces informations en tant que paramètres.

Par exemple, si vous souhaitez démarrer une activité *AfficherMessageActivity* (appartenant à votre application de messagerie) pour afficher un message, vous pouvez utiliser le code suivant :

{% highlight bash %}
Intent messageIntent = new Intent(this, AfficherMessageActivity.class);
startActivity(messageIntent);
{% endhighlight%}
<br/>

Le constructeur **Intent()** prend deux arguments pour une intention explicite :

* Le *contexte* de l’intent : ici, c’est le mot réservé *this* et cela représente l'activité appelante)

* La *cible* : ici, l'activité ciblée est AfficherMessageActivity.


La méthode **startActivity()**  prend en argument l’objet *messageIntent* et l’envoie au système Android. Android excécute une instance AfficherMessageActivity, pour votre application. La nouvelle activité apparaît sur l'écran et l'activité appelante est mise en pause.

<br/>

## Les **Intents implicites**

L'autre forme d'*intent* est celle qui ne cible aucune activité en particulier. Vous déclarez l’action à exécuter à travers l’Intention, et le système du téléphone se charge de scanner toutes les applications installées là-dessus, et d'y retrouver les activités appropriées capabes d'exécuter le type d’action spécifiée **et qui sont publiquement accessibles**.