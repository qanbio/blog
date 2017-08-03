---
layout: post
title: Stockage de données en local pour une application android 1/2
categories: [tech]
tags: [android, java, realm, sharedPreferences,Storage,Internal Storage,External Storage,database,SQLite]
author: crepin_fadjo
comments: true
fullview: false
description: Cet article va vous permettre de cerner les différents moyens pour stocker vos données en local pour une application android.
---
### Problématique :
Certaines applications Android nécessite de persister des données en local.C'est le cas par exemple de la couleur préférée de l’utilisateur, ses paramètres, des fichiers téléchargés sur internet ou chargé depuis son téléphone. Nous allons explorer dans ce qui suit, les différentes alternatives offertes par l’écosystème Android.


### Pré requis 

* Etre débutant ou developpeur professionnel d'application android;
* Utliser android Studio comme IDE(Environnement de Developpement Intégré);
* Connaitre java.

###  Le stockage de données en local  

Pour stocker les données en local Android nous fournit plusieurs possiblités à savoir :
* les données partagées (Shared Preferences),
* le stockage interne (Internal Storage) ,
* le stockage externe (External Storage),
* les bases  de données locales :
    *  Celle proposée par Android (SQLite),
    *  Et les autres (Realm ..).  
    Dans la première partie de cet article nous  parlererons des Shared Preferences, de l'Internal Storage et de l'External Storage.


### Shared Preferences

Les Shared Preferences permettent de stocker des données par clé-valeur par l'intermédiaire de la Classe **sharedPreferences**. L'avantage réel d'utiliser les Shared Preferences est que les données stockées sont conservées même quand l'application est arrêtée ou tuée.


#### Comment  utiliser les Shared Preferences?  
Pour avoir accès au Shared Preferences  nous avons principalement  deux méthodes:

*  **getPreferences(int mode)**

{% highlight bash %}
SharedPreferences preferences= getPreferences(MODE_PRIVATE) ;
{% endhighlight bash %}
Utiliser ceci quand vous voulez utiliser un seul fichier pour stocker vos préférences.

* **getSharedPreferences(String filename,int mode)**
{% highlight bash %}
SharedPreferences preferences=getSharedPreferences(myFile, MODE_PRIVATE);
{% endhighlight bash %}
Utiliser ceci quand vous voulez utliser plusieurs fichiers pour stocker vos préférences.

##### Que signifie le paramètre (int mode)?
Cela correspond au mode d'accès des fichiers de sharedPreferences créés. Ainsi nous avons les modes:
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
Récupérons le String enrégistré précedemment.
{% highlight bash %}

public String value=preferences.getString("MystringKey","DefautValue");

{% endhighlight bash %}
Avec DefaultValue une valeur par défaut qu'il faut spécifier au cas où la valeur recherchée n'ait pas été trouvée.

NB: les Shared Preferences ne fonctionnent qu'avec les objets de type boolean, float, int, long et String.

Un cas pratique est disponible sur mon compte github via ce lien.



### Le stockage interne (Internal Storage)
Pour stocker des données (fichiers) on peut aussi utiliser le stockage interne de notre téléphone.
Par défaut les fichiers  sauvegardés  dans le stockage interne sont privés à l'application.
Quand l'utilisateur désinstalle l'application, les fichiers sont automatiquement supprimés.


#### Comment sauvegarder un fichier dans le stockage interne ?
Pour créer et écrire dans un fichier privé en stockage interne il faut:
*   Appeler **openFileOutput(String fileName,int mode)**  avec fileName le nom du fichier et mode le mode d'accès : décrit un peu plus haut;
*   Utiliser **write()** pour écrire dans le fichier;
*   Fermer le flux d'écriture avec **close()**.

Voici un exemple qui montre comment créer et sauvegarder un fichier contenant une chaine de caractère

{% highlight bash %}

 private void saveInterne() throws IOException {

            FileOutputStream outputStream= openFileOutput(MY_FILE_NAME,MODE_PRIVATE);
            String audio_name="Best music for Africa";
            outputStream.write(audio_name.getBytes());
            outputStream.close();

    }

{% endhighlight%}


#### Comment lire les données contenu dans un fichier sauvegarder en interne ?

Pour lire le contenu d'un fichier interne il faut:
* Appeler **openFileInput(String fileName)** avec fileName le nom du fichier à récupérer;
* Utiliser **read()** pour lire les bytes à partir du fichier;
* Fermer le flux d'écriture avec **close()**.

Voici un exemple qui montre la lecture du fichier créé précédemment

{% highlight bash %}

    private String getFromInterne() throws IOException {
        String value = null;

        FileInputStream inputStream=openFileInput(MY_FILE_NAME);
        StringBuilder stringb= new StringBuilder();
        int content;
        while ((content=inputStream.read())!=-1){
          value = String.valueOf(stringb.append((char)content));
        }

        return value ;
    }

{% endhighlight%}


### Le stockage Externe (External Storage)  


<<<<<<< e002c4bc28d63719e7c8c50d96a442283b80a047
Chaque appareil Android prend en charge un stockage externe que nous pouvons utiliser pour stocker des données. Mais le problème avec le stockage externe est que l'utilisateur a accès au fichier et peut donc les déplacer ou les supprimér quand il veut. Par contre on a une grande capacité de stockage. Le stockage externe peut être une carte SD ou une partie du stockage interne dédiée pour cela.
=======
Chaque appareil Android prend en charge un stockage externe que nous pouvons utiliser pour stocker des données. Mais le problème avec le stockage externe est que l'utilisateur a accès au fichier et peut donc les déplacer ou les supprimer quand il veut. Par contre on a une grande capacité de stockage. Le stockage externe peut être une carte SD ou une partie du stockage interne dédiée pour cela.
>>>>>>> partie 2 de l'article en cours

 

#### Comment utliser le stockage externe ?
Pour lire ou écrire des fichiers dans le stockage externe l'application doit:

 1. déclarer les permissions **READ_EXTERNAL_STORAGE** ou **WRITE_EXTERNAL_STORAGE** dans le fichier manifest :

 {% highlight bash %}
<manifest ...>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    ...
</manifest>
{% endhighlight%}
ou

{% highlight bash %}
<manifest ...>
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    ...
</manifest>
{% endhighlight%}
2. Vérifier la disponibilité du stockage grâce à **getExternalStorageState()**;
3. Sauvegarder ou récupérer des fichiers  


Pour lire ou écrire des fichiers dans le stockage externe l'application doit:
Voici un exemple qui montre comment créer et sauvegarder un fichier contenant une chaine de caractère dans le dossier download de votre téléphone



{% highlight bash %}

//vérifier si la mémoire externe est disponible et qu'on peut écrire dessus
 public boolean isExternalStorageWritable() {
        String state = Environment.getExternalStorageState();
        if (Environment.MEDIA_MOUNTED.equals(state)) {
            Toast.makeText(this, "memoire externe disponible en écriture", Toast.LENGTH_SHORT).show();
            return true;

        }
        return false;
    }


     private void writeFile() {

        if(isExternalStorageWritable()) {

            //choisir le dossier Download comme dossier de stockge du fichier
            File extStore = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOWNLOADS);
            
            //chemin du fichier incluant le nom du fichier
            String path = extStore.getAbsolutePath() + "/" + fileName;
            
            // le message a écrit ans le fichier
            String data = "I save something here";
          
          //création du fichier
            try {
                File myFile = new File(path);
                FileOutputStream fOut = new FileOutputStream(myFile);
                OutputStreamWriter myOutWriter = new OutputStreamWriter(fOut);
                myOutWriter.append(data);
                myOutWriter.close();
                fOut.close();

                Toast.makeText(getApplicationContext(), fileName + " saved", Toast.LENGTH_LONG).show();
            } catch (Exception e) {
                e.printStackTrace();
            }
       }else{
           Toast.makeText(this, "Disque externe non disponible,ou non diponible en écriture", Toast.LENGTH_SHORT).show();
        }
    }

{% endhighlight%}


Lecture du fichier créé :

{% highlight bash %}

 public boolean isExternalStorageReadable() {
        String state = Environment.getExternalStorageState();
        if (Environment.MEDIA_MOUNTED.equals(state) ||
                Environment.MEDIA_MOUNTED_READ_ONLY.equals(state)) {
            Toast.makeText(this, "memoire externe disponible en lecture", Toast.LENGTH_SHORT).show();
            return true;
        }
        return false;
    }

     private void readFile() {

        if(isExternalStorageReadable()) {

            File extStore = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOWNLOADS);
            // ==> /storage/emulated/0/note.txt
            String path = extStore.getAbsolutePath() + "/" + fileName;
            Log.i("ExternalStorageDemo", "Read file: " + path);

            String s = "";
            String fileContent = "";
            try {
                File myFile = new File(path);
                FileInputStream fIn = new FileInputStream(myFile);
                BufferedReader myReader = new BufferedReader(
                        new InputStreamReader(fIn));

                while ((s = myReader.readLine()) != null) {
                    fileContent += s + "\n";
                }
                myReader.close();

                //this.textView.setText(fileContent);
            } catch (IOException e) {
                e.printStackTrace();
            }
            Toast.makeText(getApplicationContext(), fileContent, Toast.LENGTH_LONG).show();
        }else{
            Toast.makeText(this, "la mémoire externe n'est pas lisible", Toast.LENGTH_SHORT).show();
        }
    }

    {% endhighlight%}

### Conclusion  
 
<<<<<<< e002c4bc28d63719e7c8c50d96a442283b80a047
Android nous offre différentes possibilités pour stocker nos fichiers en local. Dans la première partie de cet article on a parlé des Shared Preferences, du stockage interne et du stockage externe. On a donc remarqué que les Shared Preferences sont souvent utilisé pour stocker des paramètres utilisateur comme le thème et autre; le stockage interne pour sauvegarder des données dont l'utilisateur n'aura pas accès et le stockage externe pour sauvegarder les fichiers un peu plus volumineux comme le son, les images, la vidéo.    
=======
Android nous offre différentes possibilités pour stocker nos fichiers en local. Dans la première partie de cet article on a parlé des Shared Preferences, du stockage interne et du stockage externe. On a donc remarqué que les Shared Preferences sont souvent utilisés pour stocker des paramètres utilisateur comme le thème et autre; le stockage interne pour sauvegarder des données auxquelles l'utilisateur n'aura pas accès et le stockage externe pour sauvegarder les fichiers un peu plus volumineux comme le son, les images, la vidéo.    
>>>>>>> partie 2 de l'article en cours
Rendez-vous dans la deuxième partie de cet article pour parler de SQLite et de Realm Database.


