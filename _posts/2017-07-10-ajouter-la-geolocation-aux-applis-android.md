---
layout: post
title: Ajouter la géolocation à votre application Android
categories: [tech]
tags: [android, java, géolocalisation, location API, android studio, application mobile, google, développement]
author: fabrice_kiki
comments: true
fullview: false
description: Intégrer la géolocalisation à votre application mobile Android, pour apporter aux utilisateurs une expérience plus authentique et réaliste.
---
### Problématique :
Rendre votre application Android, plus utile et intuitive, en proposant aux utilisateurs des informations et services, relatifs à leur position géographique et à tout ce qui se passe autour d'eux.

### Pré requis
Avoir une certaine connaissance de l’IDE (Envionnement de développement Intégré) [Android Studio](https://developer.android.com/studio/index.html). Intégrer à votre projet, une API de localisation fournie par Google Play Services (G.P.S.) , en ajoutant ce qui suit au fichier app.gradle : {% highlight bash %}compile 'com.google.android.gms:play-services-location:11.0.2'{% endhighlight%}. Intégrer un gestionnaire de permissions à la volée pour Android M et plus, en ajoutant ce qui suit au fichier app.gradle : {% highlight bash %}compile 'com.karumi:dexter:4.1.0'{% endhighlight%}. Le code source relatif à billet est disponible [ici]().

### Connaitre la position actuelle de l'utilisateur
Pour connaitre la position actuelle de l'utilisateur, votre application devra identifier la **récente position géographique connue** (the Last Known Location) occupée par le périphérique, sur le globe terrestre. Identifier cette position pour un appareil android, équivaut à reconnaitre __a priori__ la position actuelle de son utilisateur. Votre application fera donc appel au fournisseur de localisation fusionnée (__Fused Location Provider__) de G.P.S.

#### Spécifier les permissions pour l'application
Pour commencer, il faut d'abord réclamer la permission d'accès aux positions géographiques de l'appreil, pour le compte de votre application. Faites comme suit,dans le fichier Manifest.xml :

![Réclamer la permission de localisation](../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/add_location_permissions_to_manifest.PNG "Réclamer la permission de localisation")

La permission ACCESS_COARSE_LOCATION permet d'obtenir une précision géolocalisée à l'échelle des villes. Vous pouvez aussi utiliser ACCESS_FINE_LOCATION. Si le système du périphérique est Android 6.0 ou supérieur, et que le SDK cible de votre application est égal ou supérieur à 23, alors votre application doit répertorier les autorisations dans le manifeste et demander ces autorisations au moment de l'exécution (voir [code source] (https://)).


#### Créer un client de localisation
La prochaine étape consiste à créer une instance de type **FusedLocationProviderClient**, dans la méthode onCreate() de votre activité. Imitez ce qui suit :

![Créer un client de localisation](../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/create__fuse_location_provider_instance.PNG "Créer un client de localisation")


#### Obtenir la récente position de l'appareil d'un utilisateur
Une fois le client de localisation créé, il suffit d'appeler la méthode __getLastLocation()__, pour déterminer la position actuelle de l'utlisateur. A cette méthode est rattachée une taupe, qui se déclenche lorsque la position est déterminée avec succès. En cas de succès, un objet de type Location est retourné, à partir duquel vous pourrez récupérer les coordonnées de latitude et de longitude de  l'emplacement géographique actuel de l'utilisateur.

![Obtenir la position actuelle](../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/call_get_last_location.PNG "Obtenir la position actuelle")
 

### Ajuster les paramètres de localisation
Dans la majorité des cas, une application de geolocalisation, invitera l'utilisateur à adjuster les paramètres de GPS ou Wi-Fi de son appareil, si nécessaire. C'est une démarche recommandée pour éviter des situations de crash, pour votre application.  Pour effectuer une telle demande, il faut :
* Créer une requete de localisation
* Récupérer et vérifier la configuration actuelle de l'appareil
* Inviter l'utilisateur à modifier la configuration de son appareil

#### Créer une requête de localisation
Créer une instance de type **LocalRequest** avec les contraintes telles que le niveau de précision (__setPriority()__), les intervalles en millisecondes pour le rafraichissement de la position (__setInterval()__ et __setFasterIntervall()__) :

![Créer une requete de localisation](../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/setup_location_request.PNG "Créer une requete de localisation")


#### Récupérer et vérifier la configuration actuelle
Pour récupérer, les détails de la configuration actuelle du prériphérique d'un utilisateur, il faut instancier la classe **LocationSettingsRequest.Builder** et lui ajouter un ou plusieurs objets LocalRequest. Ensuite, on vérifie si la configuration retrouvée sur l'appareil de l'utilisateur est convenable pour les besoins de l'application :

![Récupérer et vérifier la configuration actuelle](../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/recuperer_et_verifier_configuration.PNG "Récupérer et vérifier la configuration actuelle")


#### Inviter l'utilisateur à modifier la configuration de son appareil
Pour inviter l'utilisateur à mofidier ses paramètres, l'application doit identifier l'état (__LocationSettingsResponse__) de la configuration du périphérique à à l'aide de deux taupes : la taupe __OnSuccessListener__ qui s'activent lorsque les paramètres de configurations de l'appareil sont favorable à la localisation, et une autre __OnFailureListener__, qui se déclenche dans le cas contraire. Lorsque tout est Ok, l'application fait la requête de localisation. En cas de pépin, une boite  de dialogue invite l'utilisateur à adjuster lui-même la configuration de son téléphone pour que sa position actuelle soit trouvée avec le GPS intégré à son téléphone. Voici le bloc de code qui exécute ce qui est expliqué :

{% highlight bash %}task.addOnSuccessListener(this, new OnSuccessListener<LocationSettingsResponse>() {
    @Override
    public void onSuccess(LocationSettingsResponse locationSettingsResponse) {
        // All location settings are convenient. 
        // Call getLastLocation() method here 
        
    }
});

task.addOnFailureListener(this, new OnFailureListener() {
    @Override
    public void onFailure(@NonNull Exception e) {
        int statusCode = ((ApiException) e).getStatusCode();
        switch (statusCode) {
            case CommonStatusCodes.RESOLUTION_REQUIRED:
                // Location settings are not satisfied, but this can be fixed
                // by showing the user a dialog.
                try {
                    // Show the dialog by calling startResolutionForResult(),
                    // Then check the result in onActivityResult().
                    ResolvableApiException resolvable = (ResolvableApiException) e;
                    resolvable.startResolutionForResult(MainActivity.this,
                            REQUEST_CHECK_SETTINGS);
                } catch (IntentSender.SendIntentException sendEx) {
                    // Ignore this error.
                }
                break;
            case LocationSettingsStatusCodes.SETTINGS_CHANGE_UNAVAILABLE:
                // Location settings are not satisfied. However, we have no way
                // to fix the settings so we won't show the dialog.
                break;
        }
    }
});{% endhighlight%}

### Le code complet fonctionnelle
Voici un example complet d'activité d'une application qui identifie la position actuelle d'un utilisateur sur Android :
{% highlight bash %}
public class MainActivity extends AppCompatActivity {

    private static final int REQUEST_CHECK_SETTINGS = 920;
    private FusedLocationProviderClient fusedLocationProviderClient;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Create an instance of the Fused Location Provider Client
        fusedLocationProviderClient = LocationServices.getFusedLocationProviderClient(this);
    }

    @Override
    protected void onResume() {
        super.onResume();
        getLocationSettings();

    }

    private void getUserCurrentLocation() {
        // Manage permissions at runtime for Android M
        Dexter.withActivity(this).withPermission(Manifest.permission.ACCESS_COARSE_LOCATION)
                .withListener(new PermissionListener() {
                    @SuppressLint("MissingPermission")
                    @Override
                    public void onPermissionGranted(PermissionGrantedResponse response) {
                        getLastKnownLocation();

                    }

                    @Override
                    public void onPermissionDenied(PermissionDeniedResponse response) {
                    }

                    @Override
                    public void onPermissionRationaleShouldBeShown(PermissionRequest permission, PermissionToken token) {
                    }
                })
                .check();
    }

    @SuppressLint("MissingPermission")
    private void getLastKnownLocation() {
        // call getLastLocation
        fusedLocationProviderClient.getLastLocation().addOnSuccessListener(new OnSuccessListener<Location>() {
            @Override
            public void onSuccess(Location location) {
                if (location != null) {
                    // Display the current location on the device's screen
                    Toast.makeText(MainActivity.this, "Latitude is :" + location.getLatitude() + " and Longitude is: " + location.getLongitude(), Toast.LENGTH_SHORT).show();
                } else {
                    Toast.makeText(MainActivity.this, "Can't get user current location at the moment", Toast.LENGTH_SHORT).show();
                }
            }
        });
    }


    protected void getLocationSettings() {

        // Create a location request
        LocationRequest mLocationRequest = new LocationRequest();
        mLocationRequest.setInterval(9000);
        mLocationRequest.setFastestInterval(4000);
        mLocationRequest.setPriority(LocationRequest.PRIORITY_HIGH_ACCURACY);

        // Get current location settings
        LocationSettingsRequest.Builder builder = new LocationSettingsRequest.Builder()
                .addLocationRequest(mLocationRequest);

        // Check if current location settings are convenient
        SettingsClient client = LocationServices.getSettingsClient(this);
        Task<LocationSettingsResponse> task = client.checkLocationSettings(builder.build());

        // Add callback to location request task
        task.addOnFailureListener(this, new OnFailureListener() {
            @Override
            public void onFailure(@NonNull Exception e) {
                int statusCode = ((ApiException) e).getStatusCode();
                switch (statusCode) {
                    case CommonStatusCodes.RESOLUTION_REQUIRED:
                        // Location settings are not satisfied, but this can be fixed
                        // by showing the user a dialog.
                        try {
                            // Show the dialog by calling startResolutionForResult(),
                            // and check the result in onActivityResult().
                            ResolvableApiException resolvable = (ResolvableApiException) e;
                            resolvable.startResolutionForResult(MainActivity.this,
                                    REQUEST_CHECK_SETTINGS);
                        } catch (IntentSender.SendIntentException sendEx) {
                            // Ignore the error.
                        }
                        break;
                    case LocationSettingsStatusCodes.SETTINGS_CHANGE_UNAVAILABLE:
                        // Location settings are not satisfied. However, we have no way
                        // to fix the settings so we won't show the dialog.
                        break;
                }
            }
        });


        task.addOnSuccessListener(this, new OnSuccessListener<LocationSettingsResponse>() {
            @Override
            public void onSuccess(LocationSettingsResponse locationSettingsResponse) {
                // All location settings are satisfied. The client can initialize
                // location requests here.
                getUserCurrentLocation();
            }
        });
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == REQUEST_CHECK_SETTINGS) {
            if (resultCode == RESULT_OK) {
                getUserCurrentLocation();
            }
        }
    }
}

{% endhighlight%}


### Conclusion 
En cours de rédaction
