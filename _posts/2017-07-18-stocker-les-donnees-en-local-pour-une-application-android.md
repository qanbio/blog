---
layout: post
title: stockage de données en local pour une application android 1/2
categories: [tech]
tags: [android, java, realm, database,SQLite]
author: crepin_fadjo
comments: true
fullview: false
description: Cet article va vous permettre de cerner les différents moyens pour stocker vos données en local pour une application android.
---
### Problématique :
Au cours du developpement d'une application android,on aura besoin de stocker des données à un moment ou à un autre en local.La couleur préférée de l'utilisateur , ses paramètres,des fichiers telechargés sur internet ou charger depuis son téléphone.Quelles sont les possibilités qui nous sont offertes ?

### Pré requis
Etre débutant ou developpeur professionnel d'application android.Utliser android Studio comme IDE(Environnement de Developpement Intégré).Connaitre java.

###  Le stockage de données en local
Pour stocker les données en local Android nous fournis plusieurs possiblités à savoir :
* les données partagées (Shared Preferences),
* ,
* le stockage externe (External Storage),
* les bases  de données locales :
*  Celle proposée par Android (SQLite),
*  Et les autres (Realm ..).
le stockage interne (Internal Storage)
Dans la première partie de cet article nous  parlererons des Shared Preferences, de l'Internal Storage et de l'External Storage.


### Shared Preferences
les Shared Prefenrences permettent de stocker des données par clé-valeur par l'intermédiaire de la Classe.L'avantage réel d'utliser les shared Preferences est que les données stockées sont conservées même quand l'application est arrêtée ou tuée.

#### Comment  utiliser les Shared Preferences?  
Pour avoir accès au Shared Preferences  nous avons principalement  deux méthodes:
*  getPreferences(int mode)
{% highlight bash %}
SharedPreferences preferences= getPreferences(MODE_PRIVATE) ;
{% endhighlight bash %}
Utiliser ceci quand vous voulez utiliser un seul fichier pour stoker vos préférences.

* getSharedPreferences(String filename,int mode)
{% highlight bash %}
SharedPreferences preferences=getSharedPreferences(myFile, MODE_PRIVATE);
{% endhighlight bash %}
Utiliser ceci quand vous voulez utliser plusieurs fichiers pour stocker vos préférences.

##### Que signifie le paramètre (int mode)?
Cela correspond au mode d'accès des fichiers de sharedPreferences créé.Ainsi nous avons les modes:
* MODE_PRIVATE, pour que le fichier créé ne soit accessible que par l'application qui l'a créé.
* MODE_WORLD_READABLE,  pour que le fichier créé puisse être lu par n'importe quelle application.
* MODE_WORLD_WRITEABLE, pour que le fichier créé puisse être lu et modifié par n'importe quelle application.

#### Comment enrégistrer les données dans les SharedPreferences ?
{% highlight bash%}
public static final String PREFS_NAME = "MyPrefsFile";

// Avoir accès au SharedPreferences
 final SharedPreferences preferences=getSharedPreferences(PREFS_NAME,MODE_PRIVATE);

//appeler l'objet SharedPreferences.Editor pour ajouter ou modifier des préférences
 SharedPreferences.Editor editor=preferences.edit();
                editor.putString("MystringKey","MyString");

                //enrégistrer les modifications
                editor.commit();
{% endhighlight bash%}

#### Comment récupérer les données des SharedPreferences ?
Supposons qu'on veut récupérer le String enrégistré précedemment.
{% highlight bash %}

public String value=preferences.getString("MystringKey","DefautValue");

{% endhighlight bash %}
Avec DefaultValue une valeur par défaut qu'il faut spécifier au cas ou la valeur recherchée n'ait pas été trouvée.

NB: les Shared Preferences ne fonctionnent qu'avec les objets de type boolean, float, int, long et String.

Un cas pratique est disponible sur mon compte github via ce lien

### Le stockage interne (Internal Storage)
Pour stocker des données(fichiers) on peut aussi utliser le stockage interne de notre téléphone.
Par défaut les fichiers  sauvegardés  dans le stockage interne sont privés à l'application.
Quand l'utilisateur désinstalle l'application, les fichierrs sont automatiquement supprimés.. 



### Autres tires
texte ici

### Conclusion 
texte ici