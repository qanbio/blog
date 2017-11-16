---
layout: post
title: Débogage avec Android Studio en mode WIFI
categories: [tech]
tags: [Android, ABD, WIFI, ADB WIFI, Deboggage, Debug]
author: fabrice_kiki
comments: true
fullview: false
description: Ce article montre comment faire du débogage avec Android Studio par Wifi, sans plugin spécialisé.
---

# Prérequis
<a href="https://developer.android.com/studio/index.html?gclid=Cj0KCQjw95vPBRDVARIsAKvPd3Ljl-9BwN6bjz3QhsKkLdxyCAg9wpZzgkSsKCPhK7JfBBKxqWP6c7waAiQTEALw_wcB">Android Studio 2.3.3</a> ou une version supérieure.


# Problématique

Habituellement, le développeur Android déboge ses applications sur un smartphone connecté à son ordinateur via un câble USB. Mais il arrive bien des fois que le câble se détériore par l'usure. Pour pâlier à ce problème, il peut déboguer avec Android Studio sans utiliser un câble USB, et sans installer de plugin particulier. Tout ce qu'il lui faut c'est une connexion internet stable.

# Procédure en invite de commande

Trouver et exécuter le ficher *adb.exe* dans le sous-dossier */platform-tools* du *sdk* de Android Studio :
(Pour retrouver facilement le chemin du sdk en passant par Android Studio : *File -> Settings -> Android SDK -> Android SDK location*.)

{% highlight bash %}cd /sdk/platform-tools/{%endhighlight%}

Brancher votre smartphone à l'ordinareur en mode débogage avec un câble USB. Ça sera nécessaire jusqu'au bout du processus. Ensuite, on pourra se passer du câble.
Vérifer la liste des smartphones connectés à votre ordinateur en mode débogage :

{% highlight bash %}adb devices{%endhighlight%}
 
Voici le résultat si un appareil est déjà connecté (Si la commande n'est pas reconnue, ajouter le chemin d'accès de adb.exe, à vos variables d'environnement) :

{% highlight bash %}List of devices attached
XXXXXXXXX   device{%endhighlight%}

Définir le port TCP sur lequel le débogueur sera fonctionnel (par exemple, le port 7777:
{% highlight bash %}adb shell setprop service.adb.tcp.port 7777
adb tcpip 7777{%endhighlight%}
Voici le bon résultat :
{%highlight bash%}restarting in TCP mode port: 7777{%endhighlight%}

Vérifier l'addresse IP de votre smartphone (par exemple 192.168.9.118) . Pour cela, sur votre smartphone aller dans :
*Paramètres -> À propos du téléphone -> État -> Adresse IP*

Retourner sur l'ordinateur et executer la commande *adb connect adresseip:port*, par exemple :
{%highlight bash%}adb connect 192.168.9.118:7777{%endhighlight%}

Une fois que le débogueur a basculé sur le réseau internet, vous aurez une réponse satisfaisante dans le genre :
{%highlight bash%}connected to 192.168.9.118:7777{%endhighlight%}

Vérifer encore la liste des smartphones connectés à votre ordinateur en mode débogage avec la commande 
{% highlight bash %}adb devices{%endhighlight%}

Le résultat sera similaire à ceci :
{% highlight bash %}List of devices attached
192.168.9.118:7777      device
{%endhighlight%}


**Et voilà ! Vous êtes en mode débogage WIFI ! Vous pouvez débrancher le câble USB**