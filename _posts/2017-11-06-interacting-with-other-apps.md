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

### Intents explicites

Les *Intents* explicites sont utilsés dans les cas où l'on veut démarrer une activité dont on connait précisément le nom (et idéalement le nom de package). Il suffit de préciser ces informations en tant que paramètres.

Par exemple, si vous souhaitez démarrer une activité *AfficherMessageActivity* (appartenant à votre application de messagerie) pour afficher un message, vous pouvez utiliser le code suivant :


{% highlight bash %}
Intent messageIntent = new Intent(this, AfficherMessageActivity.class);
startActivity(messageIntent);
{% endhighlight%}


Le constructeur **Intent()** prend deux arguments pour une intention explicite :

* Le *contexte* de l’intent : ici, c’est le mot réservé *this* et cela représente l'activité appelante)

* La *cible* : ici, l'activité ciblée est AfficherMessageActivity.

La méthode **startActivity()**  prend en argument l’objet *messageIntent* et l’envoie au système Android. Android excécute une instance AfficherMessageActivity, pour votre application. La nouvelle activité apparaît sur l'écran et l'activité appelante est mise en pause.


### Les Intents implicites

L'*intent* implicite est une façon plus flexible d'utiliser les intetions en Android.

Vous n'avez pas besoin de spéfifier l'activité ou l'autre composant à exécuter. Vous déclarez simplement l’action à exécuter à travers l’intention, et le système du téléphone se charge de scanner toutes les applications installées là-dessus pour trouver celle qui peut effecuter l'action pour le compte de votre application.

Si une seule activité correspond, cette activité est lancée. S'il existe plusieurs activités correspondantes, l'utilisateur se voit proposer un sélecteur d'application qui lui permet de choisir l'application à laquelle il souhaite effectuer la tâche. **En utilisant les *intents implicites*, votre application peut donc interagir avec les autres applications d'un téléphone.**

# Dialoguer avec les autres applications
Pour envoyer une intention implicite, il faut :

1. Dans l'activité d'envoi, créez un nouvel objet Intent, SANS préciser la cible.

2. Associer l'action à exécuter pour le compte de l'objet Intent.

3. Verifier qu'il y a au moins une activité capable de répondre à l'objet Intent.

4. Envoyer l'intent au système du téléphone avec les méthodes *startActivity()* (ou *startActivityForResult()* si le resultat de l'action doit être retournée par le composant qui l'exécutera).

### Créer une instance d'Intention implicite

{% highlight bash %}Intent sendIntent = new Intent();{%endhighlight%}

Vous pouvez directement déclarer l'action à executer en paramètre pour le constructeur :

{% highlight bash %}Intent sendIntent = new Intent(Intent.ACTION_VIEW);{%endhighlight%}

L'exemple suivant montre comment joindre des données (valeurs primitive, chaines de caractères) en *extras* à l'obect Intent, pour qu'elles soient transférées à l'activité qui executera l'action associée.

{% highlight bash %}
Intent sendIntent = new Intent();
sendIntent.setAction(Intent.ACTION_SEND);
// La variable textMessage mise en paramètre pour joindre du texte en extras
sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
sendIntent.setType("text/plain");
{%endhighlight%}

Voici la [liste exhaustive](https://chromium.googlesource.com/android_tools/+/febed84a3a3cb7c2cb80d580d79c31e22e9643a5/sdk/platforms/android-23/data/activity_actions.txt) de toutes les actions qui peuvent être définies à travers une intention implicite avec Android.

## Valider une intention impicite avant l'envoi

Lorsque vous définissez une intention implicite avec une action, il se peut qu'il n'y ait aucune
activité sur l'appareil capable de gérer votre demande. Si vous envoyez une intention et qu'il n'y a pas de correspondance appropriée, votre application va planter. Pour y remédier, il faut donc vérifier qu'une activité ou un autre composant est disponible pour recevoir votre intention, AVANT d'appeler la méthode *startActivity()*.

 Pour cela, utilisez la méthode *resolveActivity()* comme suit :

{%highlight bash%}
if (sendIntent.resolveActivity(getPackageManager()) != null) {
startActivity(chooser);
}
{%endhighlight%}

Si la méthode *resolveActivity()* ne retourne pas *null*, cela signifie qu'il y au moins une application qui possède une activité en mesure de prendre en charge l'intention implicite. Et donc, vous pouvez ensuite envoyer l'intention avec *startActivity()* (ou *startActivityForResult()* si le resultat de l'action doit etre retournée par le composant qui l'exécutera).

## Afficher le sélecteur des applications

Lorsqu'il y a une multitude d'applications capable de répondre favorablement à la requête d'une intention implicite, le système Android présente à l'utilisateur, le **sélectionneur d'application**. Le sélectionneur d'applications permet à l'utilisateur de choisir volontairement l'application qui executera la tache associée à l'intention implicite déclenchée par une autre application sur son appareil.


<br/>
<div align="center">
<img src="../../../../assets/media/2017-11-06-interacting-with-other-apps/selectionneur-de-applications.png" alt="apps-interactions" height="640" width="360" ALIGN="middle">
</div>
<br/>

# Conclusion
Les applications sur Android, ne sont pas obligées d'implémenter elles-meme toutes les fonctionnalités dont elles ont besoin pour offrir à l utilisateur le service pour lequel elles sont conçues. La plateforme Android offre aux développeurs des mécanismes flexibles et efficaces pour permettre l'interopérabilité de leur applications. Le concept d'Intent représente le coeur même du développement Android, parce qu'il offre la possibilité à une application, de solliciter les resources d'une autre application tierce, pour exécuter une tâche critique à son propre service.