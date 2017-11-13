---
layout: post
title: Interagir avec d'autres applications avec les objets Intents
categories: [tech]
tags: [Android, Activity, Intent, Mobile]
author: fabrice_kiki
comments: true
fullview: false
description: Ce post montre comment faire interagir votre application avec les autres applications disponibles sur le téléphone de l'utilisateur.
---
# Prérequis
<a href="https://developer.android.com/studio/index.html?gclid=Cj0KCQjw95vPBRDVARIsAKvPd3Ljl-9BwN6bjz3QhsKkLdxyCAg9wpZzgkSsKCPhK7JfBBKxqWP6c7waAiQTEALw_wcB">Android Studio 2.3.3</a> ou une version supérieure.


# Problématique
Utiliser les [Intents](https://developer.android.com/reference/android/content/Intent.html) (intentions) pour permettre à votre application mobile d'interagir avec les autres applications et d'échanger des ressources avec celles-ci.

# Introduction

Une application Android est avant tout une collection d'[activités](https://developer.android.com/reference/android/app/Activity.html). Chaque activité représente un écran de cette application. Pour permettre à l’utilisateur de naviguer d'un écran à un autre, les applications utilisent le mécanisme  des Intent (ce qui signifie littéralement intention). En Android, la classe [Intent](https://developer.android.com/reference/android/content/Intent.html) permet à une application de fournir au téléphone, des paramètres utiles pour déclencher l’affichage d’une activité, dans le but d’effectuer des tâches précises (par exemple, démarrer une activité pour prendre une photo, une autre pour envoyer un email, et consorts).

<br/>
<div align="center">
<img src="../../../../assets/media/2017-11-06-interacting-with-other-apps/apps-interactions.png" alt="apps-interactions" height="428" width="300" ALIGN="middle">
</div>
<br/>

# Les [Intents](https://developer.android.com/reference/android/content/Intent.html)

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

L'*intent* implicite est une façon plus flexible d'utiliser les intetions en Android.

Vous n'avez pas besoin de spéfifier l'activité ou l'autre composant à exécuter. Vous déclarez simplement l’action à exécuter à travers l’intention, et le système du téléphone se charge de scanner toutes les applications installées là-dessus pour trouver celle qui peut effecuter l'action pour le compte de votre application.

Si une seule activité correspond, cette activité est lancée. S'il existe plusieurs activités correspondantes, l'utilisateur se voit proposer un sélecteur d'application qui lui permet de choisir l'application à laquelle il souhaite effectuer la tâche. **En utilisant les *intents implicites* votre application peut donc interagir avec les autres applications d'un téléphone.**

# Envoyer des intentions implicites aux autres applications
Pour envoyer une intention implicite, il faut :

1. Dans l'activité d'envoi, créez un nouvel objet Intent sans préciser la cible.

2. Associer l'action à exécuter pour le compte de l'objet Intent.

3. Verifier qu'il y a au moins une activité capable de répondre à l'objet Intent.

4. Envoyer l'intent au système du téléphone avec les méthodes *startActivity()* ou *startActivityForResult()*.

## Créer une instance d'Intention implicite

{% highlight bash %}Intent sendIntent = new Intent();{%endhighlight%}
<br/>
Vous pouvez directement déclarer l'action à executer en paramètre pour le constructeur :

{% highlight bash %}Intent sendIntent = new Intent(Intent.ACTION_VIEW);{%endhighlight%}

L'exemple suivant montre comment joindre des données (valeurs primitive, chaines de caractères) en *extras* à l'obect Intent, pour qu'elles soient transférées à l'activité qui executera l'action associée.

{% highlight bash %}

Intent sendIntent = new Intent();
sendIntent.setAction(Intent.ACTION_SEND);
// La variable textMessage mis en paramètre pour joindre du texte
sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
sendIntent.setType("text/plain");

{%endhighlight%}

Ici se trouve la [liste exhaustive](https://chromium.googlesource.com/android_tools/+/febed84a3a3cb7c2cb80d580d79c31e22e9643a5/sdk/platforms/android-23/data/activity_actions.txt) de toutes les actions qui peuvent être définies à travers une intention implicite avec Android.

<br/>

## Valider une intention impicite avant l'envoi

## Afficher le sélecteur des applications

# Recevoir des intentions implicites

# Conclusion
