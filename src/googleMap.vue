<template>
    <div class="google-map" :id="mapName">
         <div>{{properties}}</div>
    </div>
   
    
</template>

<script>
    export default {
        name: 'google-map',

        props: ['name'],

        data() {
            return {
                mapName: this.name + "-map",
                    properties: "privates",

                      watch: {
                       properties() {
                        console.log("I am changed:", this.properties)
                        }
                    }
            }
  
        },
        mounted() {
            const element = document.getElementById(this.mapName);
            const options = {
                zoom: 14,
                center: new google.maps.LatLng(51.501527,-0.1921837)
            }
            const map = new google.maps.Map(element, options);

            map.addListener('click', function(event) {
                console.log("evventtt", event)
                const lat = event.latLng.lat();
                const long = event.latLng.lng();
                const coordinates = {"latitude": lat, "longitude": long};
                console.log("THESE ARE MY COORDINATES:", coordinates);
                const a = 0.025;
                const b = 0.025;
                const latMax = lat + a;
                const latMin = lat - a;
                const longMax = long + b;
                const longMin = long - b;
                const boundingCoordinates = {"latitudeMax": latMax, "latitudeMin": latMin, "longitudeMax": longMax, "longitudeMin": longMax} 
                console.log("these are the LatMax, LatMin, LongMax, LongMin",boundingCoordinates);

                const endMarker = new google.maps.Marker({
                    position: new google.maps.LatLng(lat,long),
                    map: map,
                    draggable: false,
                });
                const p1 = new google.maps.Marker({
                    position: new google.maps.LatLng(latMax,long),
                    map: map,
                    draggable: false,
                });
                console.log("THIS IS P1:", p1.position);
                
                   const p2 = new google.maps.Marker({
                    position: new google.maps.LatLng(latMin,long),
                    map: map,
                    draggable: false,
                });
                   const p3 = new google.maps.Marker({
                    position: new google.maps.LatLng(lat,longMax),
                    map: map,
                    draggable: false,
                });
                   const p4 = new google.maps.Marker({
                    position: new google.maps.LatLng(lat,longMin),
                    map: map,
                    draggable: false,
                });

                 getProperties(latMin,latMax,longMin,longMax);

                function getProperties(latMinCoord,latMaxCoord,longMinCoord,longMaxCoord) {
                    console.log("Hello, I work!");
                    const axios = require('axios');
                    let baseURL = "http://api.zoopla.co.uk/api/v1/property_listings.js";
                    let apiKey = "api_key=";
                    let apiKeyValue = "p6b66f54ehddddjkd7kkm7ec";
                    let latMinUrl = "lat_min=";
                    let latMaxUrl  = "lat_max=";
                    let longMinUrl  = "lon_min=";
                    let longMaxUrl  = "lon_max=";
                    let url = baseURL + "?" + apiKey + apiKeyValue + "&"+ latMinUrl + latMinCoord + "&" + latMaxUrl + latMaxCoord + "&" + longMinUrl + longMinCoord +"&" + longMaxUrl + longMaxCoord;
                    console.log("THIS IS MY URL:",url);
                    axios.get(url)
                    .then(response => {
                        console.log("THIS IS MY RESPONSE",response);
                        this.properties = response.data.result_count;
                    })
                    .catch(function (error) {
                        console.log("THIS IS THE ERROR:", error);
                    });
                }
            })
        },

        methods: {
     
        }
    }
</script>

<style>
.google-map {
  width: 800px;
  height: 600px;
  margin: 0 auto;
  background: gray;
}
</style>
