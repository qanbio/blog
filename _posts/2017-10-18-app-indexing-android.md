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

Permettre à Google Search de présenter votre application Android comme destination, lorsque l'utilisatieur clique sur un lien ou un résultat de recherche, dont le contenu correspond à votre site web.

# Solution

Ajouter des **App Links** Android à votre application en suivant les étapes suivantes :

* Définir des URLs appelés Deep Links, qui ouvrent le contenu précis de votre application grâce aux Intent Filters.
* Prouver que vous êtes effectivement le propriétaire, à la fois votre application et des URL du site Web.

Dans le présent article, nous allons nous focaliser sur comment ajouter les Intent Filters, et comment définir les Deep Links avec Android Studio.

# Concept de App Link et Deep Link

Avant d'aller plus loin dans le post, clarifions les concoepts de **Deep links** et **App Links** pour Android.

## Deep Links (Liens profonds)

Ce sont des URL, qui amènent  l'utilisateur directement vers un contenu spécifique dans votre application (ex: affichage d'un text, d'une image, d'une vidéo, etc...). En Android, il est possible de définir des Deep Links en ajoutant des <a href="https://developer.android.com/guide/components/intents-filters.html">Intents Fiters</a> puis, d'extraire des informatiosn utiles des Intents appelant votre application, pour déterminer l'activité convenable à lancer.

## Android App Links

Introduits à partir de la version Marshmallow de l'OS Android, les App Links permettent à une application d'être désignée par l'utilisateur comme étant l'application par défaut à lancer pour ouvrir un lien sur le téléphone. Et si  l'utilisateur ne désire plus utiliser une application comme application par défaut, il peut toutefois la révoquer dans les paramètres de son téléphone.
Ajouter des App Links à une application présente plusieurs avantages :

* Les liens que vous indexez dans votre application sont ceux de votre site web
* Les utilisateurs qui n'ont pas accès à votre site web, peuvent toutefois avoir le contenu recherché sur leur téléphone
* Les utilisateurs sont plus engagés parce qu'ils accècent directement au contenu de votre application, depuis le moteur de recherche Google. Ils n'ont pas besoin de passer par un protocole  connexion pour les contenus généraux et publics.

# Créer des Deep Links avec des Intent Filters

Un **Intent Filter** est une expression définie dans le fichier manifest.xml d'une application, pour spécifier le format de requêtes ( implicites ou explicites) qui vont déclencher l'activation d'un composant (généralement une activité ou un service) dans cette application.
Dans le contexte des Deep Links, un Intent Filter est défini pour mapper une URL à une activité. L'Intent Filter doit renseigner les attributs suivants :

## Le tag [action](https://developer.android.com/guide/topics/manifest/action-element.html)

Le tag ACTION fera que l’Intent Filter sera accessible depuis Google Search. La valeur est [android.intent.action.VIEW](https://developer.android.com/reference/android/content/Intent.html#ACTION_VIEW).

## Le tag [data](https://developer.android.com/guide/topics/manifest/data-element.html)

Le tag DATA définit l’URI précis auquel sera mappé l’activité de votre application. Le tag data doit inclure les attributs suivants :

* L'attribut [android:scheme](https://developer.android.com/guide/topics/manifest/data-element.html#scheme)

L'attribut renseigne le nom du schéma de l’URI. La valeur est soit **http** ou bien **https**. Cet attribut est obligatoire.

* L'attribut [android](https://developer.android.com/guide/topics/manifest/data-element.html#host)

Cet attribut renseigne le nom de domaine. Par exemple, le nom de domaine est [www.qanbio.com](http://www.qanbio.com). Cet attribut est obligatoire.

* L'attribut [android:path](https://developer.android.com/guide/topics/manifest/data-element.html#path) :

Ce attribut précise le chemin de l’URI qui pointe vers l’activité. Pour notre exemple, le préfix est : ~/home~. Le chemin de l’URI doit toujours commencer par un slash “/”

* L'attribut [android:category](https://developer.android.com/guide/topics/manifest/category-element.html) :

Ajouter la valeur **android.intent.category.BROWSABLE** à cet attribut, pour que l’activité associée au Filter Intent, soit prise proposée à l’utilisateur comme action, lorsqu’il aura cliqué sur l’URI mappée dans un navigateur web ou dans une autre application. Ajoutez également une catégorie de type.

Incluez également la catégorie **android.intent.category.DEFAULT**. Cela permet à votre application de répondre aux intentions implicites. Sans cela, l'activité ne peut être démarrée que si l'intention spécifie le nom de votre composant d'application.

Voici en guise d’exemple, l’intent Filter ajoutée à une activité “Welcome”, pour créer un Deep Link avec l’URI (http://www.qanbio.com)


{% highlight bash %}<activity
   android:name=".Welcome"
   android:label="@string/title_activity_welcome"
   android:theme="@style/AppTheme.NoActionBar">

   <intent-filter>
       <action android:name="android.intent.action.VIEW" />
       <category android:name="android.intent.category.DEFAULT" />
       <category android:name="android.intent.category.BROWSABLE" />
       
    <!-- Accepter les URIs qui commencent par l’URL "http://www.qanbio.com/welcome" -->
       <data
           android:scheme="http"
           android:host="www.qanbio.com"
    <!-- noter que le caractère "/" is obligatoire en début pour l’attribut path -->
           android:path="/welcome" />
   </intent-filter>
</activity>
{% endhighlight%}

# Gérer les appels de votre Deep Link

Une fois que le système a démarré votre activité à l'aide d'un Intent Filter, vous pouvez utiliser les données fournies par l’Intent pour déterminer le résultat à afficher. Appelez les méthodes [getData ()](https://developer.android.com/reference/android/content/Intent.html#getData()) et [getAction ()](https://developer.android.com/reference/android/content/Intent.html#getAction()) pour extraire les données et les actions associées à l'intention entrante.
Ces deux méthodes peuvent être appelées a n’importe quelle phase  du [cycle de vie](https://developer.android.com/guide/components/activities/activity-lifecycle.html) de l'activité. Mais les phases recommandées sont  **onCreate ()** ou **onStart ()**, et **onNewIntent()**.

Dans l'exemple ci-dessous, la méthode  handleIncomingIntent(), récupère l'URL à partir de laquelle, l'activité a été appelée. Cette méthode sera exécutée durant la phase de création de l'activité (méthode _onCreate()_), et à chaque nouvel appel entrant (méthode _onNewIntent_):


{% highlight bash %}
@Override
protected void onCreate(Bundle savedInstanceState) {
   super.onCreate(savedInstanceState);
   setContentView(R.layout.welcome);
   Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
   setSupportActionBar(toolbar);
   webView = (WebView) findViewById(R.id.webview);
   if (getSupportActionBar() != null) {
       getSupportActionBar().setDisplayHomeAsUpEnabled(true);
    }
   handleIncomingIntent(getIntent());
}

@Override
protected void onNewIntent(Intent intent) {
   super.onNewIntent(intent);
   handleIncomingIntent(intent);
}

private void handleIncomingIntent(Intent intent) {
   String appLinkAction = intent.getAction();
   Uri appLinkData = intent.getData();
   if (Intent.ACTION_VIEW.equals(appLinkAction) && appLinkData != null) {
       String path = appLinkData.getLastPathSegment();
       //...ajouter le code  approprié à votre logique ici
   }
}
{% endhighlight%}


# Tester votre Deep Link avec ADB

Place au teste. Utilisez le programme Android Debug Bridge (ADB) pour tester que les Deep links  associés vos actitivés sont fonctionnels et que vos Intent Filters ont été bien paramétrés. 
Vous pouvez exécuter la commande adb sur un périphérique ou un émulateur.

**Voici le formattage de la commande**

{% highlight bash %}
adb shell am start -a android.intent.action.VIEW -c android.intent.category.BROWSABLE -d < mettre ici l’URI à tester sans les chevrons >
{% endhighlight%}

**Exampples**
Par exemple, pour tester le Deep Link de l’URI [http://www.qanbio.com/welcome](http://www.qanbio.com/welcome), il faut executer la commande :
{% highlight bash %}adb shell am start -a android.intent.action.VIEW -c android.intent.category.BROWSABLE -d http://www.qanbio.com/welcome{% endhighlight%}

Si le mappage d'URL a été bien élaboré,  Android Studio lance votre application dans le périphérique ou l'émulateur à l'activité spécifiée et affiche la boîte de dialogue des actions. Dans cette boîte de dialogue, vous verrez le nom de votre activité en liaison avec l’URI mappée. Sélectionnez le nom de votre activité et elle sera lancée directement.

<img src="../../../../assets/media/2017-10-18-app-indexing-android/qanbio-app-indexing.gif" alt="Demo" height="512" width="390">

**NB :**


# Conclusion
