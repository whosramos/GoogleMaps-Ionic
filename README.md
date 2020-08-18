# GoogleMaps with Ionic 4
How to add Google Map and Multiple Marker using Ionic 4

In most of the apps, maps are one of the most useful tools for users when included in an app. The Ionic allows us to integrate Google map in our application. We can achieve integrating Google map in Ionic in two-way

* Google Map JS library
* Ionic native SDK map
In this example, we will demonstrate how to use Google Map JS library. We will cover setting up the Google Maps API through the Google Developer Console, displaying a map with multiple markers in your applications, displaying the museum’s location. We are creating an application called museumGoogleMap to demonstrate

What are we learning?

* Displaying individual map for each museum.
* Displaying multiple markers on single Google map.
* Search box to search museum by state.


## STEP #1: SETUP THE PROJECT

´´´js
ionic start  museumGoogleJsMap  blank --type=angular
´´´

## Step 2: Setting up Google  Map API in Google Developer Console

In order to use the Google Maps API, you must register your application on the Google Developer Console and enable the API. To do this, start by going to the Google Developer Console for Google JS API. If you already have a project created, you can skip the next section. If not, you can follow along and create a new project for your maps application and then you have to create credentials in Google developer console.


On clicking on CREATE AND ENABLE API, you will get API to use Google map in our project and copy the API and the past in our Ionic project src/theme/index.html at end of last line in header tag as

´´´js
<script src="https://maps.googleapis.com/maps/api/js?key=YOURGOOGLEAPI&callback=initMap"
 async defer></script>
 ´´´
 
## STEP 3: INSTALLING GEOLOCATION PLUGIN
Now we will add the Geolocation plugin, run the following command
´´´js
>>ionic cordova plugin add cordova-plugin-geolocation
>>npm install @ionic-native/geolocation --save
´´´

## STEP 4: LET’S BUILD THE APPS, FOLLOWING ALL THE STEPS AS 
We have completed the external configuration, now time to write code for our museum Ionic Apps. We will add two more page.

´´´js
ionic generate page museum-detail
ionic generate page all-museum
´´´

In **app.module.ts** file we need to register the provider for Geolocation plugin.

´´´js
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { RouteReuseStrategy } from '@angular/router';

import { IonicModule, IonicRouteStrategy } from '@ionic/angular';
import { SplashScreen } from '@ionic-native/splash-screen/ngx';
import { StatusBar } from '@ionic-native/status-bar/ngx';

import { AppComponent } from './app.component';
import { AppRoutingModule } from './app-routing.module';
import { Geolocation } from '@ionic-native/geolocation/ngx';

@NgModule({
  declarations: [AppComponent],
  entryComponents: [],
  imports: [BrowserModule, IonicModule.forRoot(), AppRoutingModule],
  providers: [
    StatusBar,
    SplashScreen,
    { provide: RouteReuseStrategy, useClass: IonicRouteStrategy },
    Geolocation
  ],
  bootstrap: [AppComponent]
})
export class AppModule {}
´´´

## CREATING MUSEUM MODEL
Inside app folder create a new folder called models and add the new file for museum model as a **src/models/museum.ts**. Add the following code

´´´js
export interface Museum{
    name: string;
    state : string;
    latitude: any;
    longitude:any
}
´´´

## STEP 5: CREATE SERVICES TO SHARED DATA BETWEEN PAGES.
 

Create a folder called service in app folder, as ionic 4 we are sharing data through services. Generate service to share museum data between pages.
>> **ionic generate service museumData**
Run above command in app/service folder and add following file in app/service

´´´js
import { Injectable } from '@angular/core';
import { Museum } from '../model/museum';

@Injectable({
  providedIn: 'root'
})
export class MuseumDataService {
  museums: [];
  museum: Museum;

  constructor() { }

  setMuseums(data) {
    this.museums = data;
  }

  getMuseums() {
    return this.museums;
  }

  setMuseum(data) {
    this.museum = data;
  }

  getMuseum() {
    return this.museum;
  }
}
´´´

## STEP 6: ADDING MUSEUM LIST AND SEARCH LIST PAGE
We have to add the list of the museum in musuem.json file. In src/assets/data add a new file called museum.json file. Add the following museum lists.
´´´json
{
  "museums": [
      {
        "name": "National Museum",
        "state" : "Delhi",
        "latitude": 28.6117993,
        "longitude": 77.2194934
      },
      {
        "name": "National Science Centre,",
        "state": "Delhi",
        "latitude": 28.6132098,
        "longitude": 77.245437
      },
      {
        "name": "The Sardar Patel Museum",
        "state": "Gujrat",
        "latitude": 21.1699005,
        "longitude": 72.7955734
      },
      {
        "name": "Library of Tibetan Works and Archives",
        "state": "Himachal",
        "latitude": 32.2263696,
        "longitude": 76.325326

      },
      {
        "name": "Chhatrapati Shivaji Maharaj Vastu Sangrahalaya",
        "state": "Maharashtra",
        "latitude": 18.926873,
        "longitude": 72.8326132
      },
      {
        "name": "Namgyal Institute of Tibetology",
        "state": "Sikkim",
        "latitude": 27.315948,
        "longitude": 88.6047829

      }
      ]
}
´´´
