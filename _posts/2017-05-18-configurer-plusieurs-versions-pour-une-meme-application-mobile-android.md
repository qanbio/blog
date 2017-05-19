---
layout: post
title: Configurer plusieurs versions pour une même application mobile android
categories: [tech]
tags: [android, java, flavors, gradle, android studio, application mobile, google, versionning, build type, apk, app variants, développement]
author: fabrice_kiki
comments: true
fullview: false
description: Développer une application Android devant être déployée en plusieurs versions (fichiers .APK) pour différentes cibles , notamment dev (pour le développement), recette (pour la recette), prod (pour la version stable et opérationnelle de l’application), avec des noms, des logos, des fonctionnalités, ou autres différences significatives.
---
### Problématique :
Développer une application Android devant être déployée en plusieurs versions (fichiers .APK) pour différentes cibles , notamment : **dev** (pour le développement), **recette** (pour la recette), **prod** (pour la version stable et opérationnelle de l’application), avec des noms, des logos, des fonctionnalités, ou autres différences significatives.

### Pré requis
Avoir une certaine connaissance de l’IDE (Envionnement de développement Intégré) [Android Studio](https://developer.android.com/studio/index.html).

### Solutions possibles
Une solution , consisterait à avoir une version originale du projet avec une multitude de duplicatas pour chaque environnement et y intégrer des modifications en parallèle. Cette méthode n'est évidemment pas du tout flexible et peut à la longue induire des complications dans la maintenance du code, au fur et à mesure que le projet prendra de l’ampleur.

Une seconde solution conisterait, dans un premier temps à élaborer une version stable avec des fonctionnalités minimales requises du projet. Puis, de créer un nouveau projet d’application pour chaque environnement. Et d’y intégrer le projet stable sous forme de librairie. Cela pourrait bien fonctionner, puisque tout le code ne sera pas répliqué. Néanmoins, au final on aura créé une application-librairie mais pas une application autonome.

Cependant, il existe une autre approche plus simple et plus flexible. Une approche avec laquelle n’importe quel développeur et son équipe peuvent gérer et déployer plusieurs versions d’une application Android avec une seule copie du projet, comme sus-cité. Cette technique est déjà intégrée dans l’IDE officiel d’Android Studio et repose sur l’outil de build Gradle [Gradle](https://gradle.org/). C’est la méthode des **Flavor** (c-à-d __saveur__ en Anglais). Les flavor permettent d’avoir une multitude de variantes d’une même application Mobile avec chacune ses éléments de différenciation, tout en assurant le maintien des points communs qui font l’originalité du projet. 

### Les Flavors : comment ça marche

Android Studio génère les variantes d’application (ou **Build Variant**) en combinant  chaque Flavor de l’application avec un “type de compilation” (ou **Build Type**).
Un type de compilation spécifie les critères de compilation et de mise en paquet (fichier APK) d’une application tels que la clé de chiffrement, l’activation du débogage, ainsi que la minification de la taille du fichier APK.  En complément, un Flavor spécifie les critères tels que les ressources à exploiter (layouts, code java, images), les versions OS compatibles, ou  les dimensions d’écran autorisées. Par défaut, Android Studio spécifie automatiquement deux types de compilation : debug (débogage) et release (production), pour chaque projet d’application.

Par exemple, En combinant deux types de build “debug”  et “release” avec les flavors “dev”, “recette” et “prod”, on obtiendra les variantes suivantes pour une même application :

* devDebug
* devRelease
* recetteDebug
* recetteRelease
* prodDebug
* prodRelease

Nous allons créer une simple application avec les trois flavor sus-indiqués, et les exploiter pour personnaliser les différentes versions du projet pour chaque environnement correspondant.
 
### Creation du Projet
Pour ce tutoriel nous utiliserons Android Studio 2.3.1 avec gradle 3.3. Cependant, les procédures décrites par la suite sont valables pour des versions antérieures et ultérieures de ces outils (sauf mention contraire).

Créons un projet Android comme à l’ordinaire, sans aucun paramétrage particulier. Nous nommons le __ThreeFlavorExample__.


### Ajouter un Flavor
Clicker ensuite sur : **Build** puis **Edit Flavors**

![ajouter un nouveau flavor](../../../../assets/media/2017-05-18-configurer-plusieurs-versions-pour-une-meme-application-mobile-android/add_new_flavor.png "ajouter un noouveau flavor")


Il est possible, pour chaque nouveau flavor, de modifier les valeurs des paramètres tels que : Min Sdk Version, Application Id, Target Sdk Version et même configurer le chiffrement :

![configurer un flavor](../../../../assets/media/2017-05-18-configurer-plusieurs-versions-pour-une-meme-application-mobile-android/add_new_flavor_details.png "configurer un flavor")

Cliquez sur OK lorsque après avoir fini de remplir les champs souhaités. Gradle fera une synchronisation du projet, après laquelle le nouveau flavor sera prêt à l’usage. On a la largesse de créer de mettre un identifiant unique (Application Id) différent pour chaque flavor. Cela garantit que toutes les versions de l’application pourront être installées sur un même appareil (pour tester au besoin).


Pour vérifier que votre nouveau flavor a été bien créé, cliquez sur l’onglet latéral **Build variants**, et vous verrez la liste des variantes possibles de votre application. Si vous sélectionnez une variante dans la liste, Gradle fera chargera les paramètres et ressources liés uniquement au Flavor et au Build Type combinés de cette variante. Ensuite, elle fera une nouvelle synchro du projet.

![variantes de compilation](../../../../assets/media/2017-05-18-configurer-plusieurs-versions-pour-une-meme-application-mobile-android/display_flavors_window.png "variantes de compilation")


Dans le cadre du présent tutoriel, nous avons créé trois flavors pour notre projet. Les déclaration de  configuration des flavors ainsi créés pour le projet se trouvent par défaut dans le fichier build.gradle du module principal __app__. Ouvrez ce fichier, pour y retrouver les blocs suivants :

{% highlight bash %}
productFlavors {
   dev {
       minSdkVersion 22
       applicationId 'com.qanbio.threeflavorexample.dev'
       targetSdkVersion 25
       versionCode 1
       versionName '1.0'
       versionNameSuffix 'dev'
   }
       recette {
       minSdkVersion 22
       applicationId 'com.qanbio.threeflavorexample.recette'
       targetSdkVersion 25
       versionCode 1
       versionName '1.0'
       versionNameSuffix 'recette'
   }

   prod {
       minSdkVersion 22
       applicationId 'com.qanbio.threeflavorexample.prod'
       targetSdkVersion 25
       versionCode 1
       versionName '1.0'
       versionNameSuffix 'prod'
   }
}
{% endhighlight%}


On peut bien créer les flavors au début ou pendant le développement du projet. 
Par défaut, il y a un dossier **main** qui contient les fichiers du projet. Le principe est d’avoir un dossier unique pour chaque flavor créé, pour  y mettre toutes les ressources spécifiques au flavor.


## Ajouter des ressources à un flavor
Il est possible de créer une ressource pour un flavor, qui *surcharge* une autre ressource de même nom. Pour l’exemple, nous allons surcharger le fichier par défaut __activity_main.xml__  contenu dan le dossier  __main__ du projet ThreeFlavorExample . Pour cela il suffit de créer un autre fichier portant le même nom, tout en prenant soin de selectionner le **source set** convenable pour le flavor ciblé.

![ajouter un fichier ressource au flavor](../../../../assets/media/2017-05-18-configurer-plusieurs-versions-pour-une-meme-application-mobile-android/add_flavor_resource.png "ajouter un fichier ressource au flavor")

Il en va de même pour les icônes :

![ajouter un fichier image  au flavor](../../../../assets/media/2017-05-18-configurer-plusieurs-versions-pour-une-meme-application-mobile-android/add_flavor_resource_image.png "ajouter un fichier image au flavor")

et les images vectorielles :     
![ajouter un fichier image vectiorielle  au flavor](../../../../assets/media/2017-05-18-configurer-plusieurs-versions-pour-une-meme-application-mobile-android/separate_flavor_resources_drawable.png "ajouter un fichier image vectorielle au flavor")


### Ajouter du code spécifique à un flavor
Contrairement aux ressources de type drawable (image, vecteurs, icônes, layouts), les fichiers .JAVA nécessitent un traitement particulier pour être répartis par flavor. Lorsque Android Studio compile un projet, il combine l’ensemble des fichiers .JAVA qui s’y trouvent. Donc pour avoir un fichier example.java spécfique à un flavor, il faut le créer directement dans le répertoire **java/nom_du_package/**. Surtout éviter d’ajouter un fichier du même nom dans le répertoire **main/java/.../**, sinon il y aura conflict.


### Conclusion 
Si vous avez des projets d’application qui se différencient par des aspects infimes, et qui sont quand même identiques du point de vue fonctionnel, alors l’usage des Flavors est recommandé. En revanche les flavors ne sont pas adéquats lorsque le même projet présentera des versions finales très différentes au niveau du code.
Par ailleurs, retenez la différence entre les Flavors et les Buil Types. Les flavors sont convenables pour créer des versions différentes d’une même application sur PlayStore, par exemple une version gratuite et une autre version payante. En revanche, les Build Types sont une option indiquée lorsqu’il s’agit de gérer différents environnements pour le même projet comme dev, debug, prod. Une compilation de type debug peut communiquer avec un serveur local contrôlé avec des données fictives de test. Alors que pour une version prod de la même application, le serveur sera différent.

