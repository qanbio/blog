---
layout: post
title: Ajouter la géolocation à votre application Android (2/2)
categories: [tech]
tags: [android, java, géolocalisation, location API, android studio, application mobile, google, développement]
author: fabrice_kiki
comments: true
fullview: false
description: Intégrer la géolocalisation à votre application mobile Android, pour proposant aux utilisateurs des informations et services, relatifs à leur position géographique et à leur environnement.
---
### Problématique :
Renouveller automatiquement les informations sur la position de l'utilisateur, en temps réel.

### Pré requis
Avoir une certaine connaissance de l’IDE (Envionnement de développement Intégré) [Android Studio](https://developer.android.com/studio/index.html). Intégrer à votre projet, une API de localisation fournie par Google Play Services (G.P.S.) , en ajoutant ce qui suit au fichier app.gradle : {% highlight bash %}compile 'com.google.android.gms:play-services-location:11.0.2'{% endhighlight%}. Intégrer un gestionnaire de permissions à la volée pour Android M et plus, en ajoutant ce qui suit au fichier app.gradle : {% highlight bash %}compile 'com.karumi:dexter:4.1.0'{% endhighlight%}. Le code source comptet opérationnel sera affiché à la fin du tutoriel.

### Démaarer l'actualisation de la position 
 Dans le [précédent billet] (<http://blog.qanbio.com/tech/2017/07/10/ajouter-la-geolocation-aux-applis-android.html>), une application utilise Google Play Services pour identifier la position actuelle d'un utilisateur, obtenue en invoquant la méthode  **getLastLocation()** sur un client de localisation de type **FusedLocationProviderClient**.

 Par ailleurs, actulaiser la position géolocalisée de l'utilisateur est particulièrement utile, lorsque l'application offre un service de suivi de livraison, ou un service d'aide à l'orientation en zone urbaine. Pour ce faire, il faut :
  * appliquer la méthode  **requestLocationsUpdates()** sur une l'instance de FusedLocationProviderClient.
  * ajouter en paramètres, une ou plusieurs positions (objets **Location**) préalablement identifiées
  * rattacher un callback de type **LocationCallback** pour récupérer les positions actualisées

  Voici une illustration
  
  ![actualiser les positions de l'utilisateur](../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/start_location_update.PNG  "actualiser les positions de l'utilisateur")

### Implémenter un callback de type LocationCallback

![implementer le callback] (../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/implement_location_callback.PNG "implementer le callback")

### Arrêter les mises-à-jour de localisation
Lorsque l'application passe en pause sur l'appareil de l'utilisateur, pour une raison pou une autre, il est recommandé de désactiver la mise-à-jour de la localisation. 

![implementer le callback] (../../../../assets/media/2017-07-10-ajouter-la-geolocation-aux-applis-android/stop_location_updates.PNG "implementer le callback")

### Exemple complet fonctionnel
Voici un exemple d'activité d'application android qui suit les changements de position de l'utlisateur sur le globe, __en temps réel__:


{%highlight bash%}

public class MainActivity extends AppCompatActivity {

    private static final int REQUEST_CHECK_SETTINGS = 920;
    LocationCallback mLocationCallback;
    private FusedLocationProviderClient fusedLocationProviderClient;
    private LocationRequest mLocationRequest;
    private boolean permissionsGranted;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Create an instance of the Fused Location Provider Client
        fusedLocationProviderClient = LocationServices.getFusedLocationProviderClient(this);

        mLocationRequest = createLocationRequest();


        mLocationCallback = new LocationCallback() {
            @Override
            public void onLocationResult(LocationResult locationResult) {
                for (Location location : locationResult.getLocations()) {
                    // Update UI with location data
                    // Display the current location on the device's screen
                    Toast.makeText(MainActivity.this, " Current location : Latitude is " + location.getLatitude() + " and Longitude is: " + location.getLongitude(), Toast.LENGTH_SHORT).show();
                }
            }
            ;
        };


    }

    @Override
    protected void onStart() {
        super.onStart();
        getLocationSettings();
    }

    private void getUserCurrentLocation() {
        // Manage permissions at runtime for Android M
        Dexter.withActivity(this).withPermissions(Manifest.permission.ACCESS_COARSE_LOCATION, Manifest.permission.ACCESS_FINE_LOCATION).
                withListener(new MultiplePermissionsListener() {
                    @Override
                    public void onPermissionsChecked(MultiplePermissionsReport report) {
                        permissionsGranted = true;
                        getLastKnownLocation();
                    }

                    @Override
                    public void onPermissionRationaleShouldBeShown(List<PermissionRequest> permissions, PermissionToken token) {

                    }
                })
                .check();
    }

    @Override
    protected void onResume() {
        super.onResume();
        if (permissionsGranted)
        startLocationUpdates(mLocationRequest, mLocationCallback);
    }

    @SuppressLint("MissingPermission")
    private void startLocationUpdates(LocationRequest locationRequest, LocationCallback mLocationCallback) {
        fusedLocationProviderClient.requestLocationUpdates(mLocationRequest, mLocationCallback, null);
    }

    @Override
    protected void onPause() {
        super.onPause();
        if (permissionsGranted)
        stopLocationUpdates(fusedLocationProviderClient);
    }

    private void stopLocationUpdates(FusedLocationProviderClient fusedLocationProviderClient) {
        fusedLocationProviderClient.removeLocationUpdates(mLocationCallback);
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

    @NonNull
    private LocationRequest createLocationRequest() {
        // Create a location request
        LocationRequest mLocationRequest = new LocationRequest();
        mLocationRequest.setInterval(9000);
        mLocationRequest.setFastestInterval(4000);
        mLocationRequest.setPriority(LocationRequest.PRIORITY_HIGH_ACCURACY);
        return mLocationRequest;
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
}{%endhighlight%}


### Conclusion 
La plateforme Android, vous permet de différencier votre application, en utiliseant la puissance de la mobilité pour donner à vos utilisateurs des informations contextuelles sur l'endroit où ils se trouvent, quand ils s'y trouvent. À vous de faire vivre à vos utilisateurs, des expériences inoubliables avec votre application. Ils vous remercieront. Bonne chance !!