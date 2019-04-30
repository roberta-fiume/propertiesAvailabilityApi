<template>
<div class="map-container">
    <div class="google-map" :id="mapName"></div>
        <h1>the total of properties is: {{numberOfProperties}}</h1>
        <h1> {{title}} </h1>
    <result-box></result-box>
  
</div>
</template>

<script>
import result from './result.vue'

    export default {
        name: 'google-map',      

        props: ['name'],

        data() {
            return {
                mapName: this.name + "-map", 
                numberOfProperties: 0,
                newProperties: 0,
                title: 'Welcome to title changer'
            }
        },

            created() {
                console.log("I AM CREATEDDDDD", this.numberOfProperties);
                if (this.numberOfProperties == "0") {
           
                }
            },

        watch: {
         
            numberOfProperties: function(val) {
                console.log("calling computed!!!!!!");
                this.numberOfProperties = val;
                console.log("I AM WATCHED:", this.numberOfProperties);
                 return this.numberOfProperties;
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

                const lat = event.latLng.lat();
                const long = event.latLng.lng();
                const coordinates = {"latitude": lat, "longitude": long};
                const a = 0.025;
                const b = 0.025;
                const latMax = lat + a;
                const latMin = lat - a;
                const longMax = long + b;
                const longMin = long - b;
          
                const boundingCoordinates = {"latitudeMax": latMax, "latitudeMin": latMin, "longitudeMax": longMax, "longitudeMin": longMax} 

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

                function getProperties(latMinCoord,latMaxCoord,longMinCoord,longMaxCoord) {
                    // const axios = require('axios');
                    let baseURL = "http://api.zoopla.co.uk/api/v1/property_listings.js";
                    let apiKey = "api_key=";
                    let apiKeyValue = "p6b66f54ehddddjkd7kkm7ec";
                    let latMinUrl = "lat_min=";
                    let latMaxUrl  = "lat_max=";
                    let longMinUrl  = "lon_min=";
                    let longMaxUrl  = "lon_max=";
                    let url = baseURL + "?" + apiKey + apiKeyValue + "&"+ latMinUrl + latMinCoord + "&" + latMaxUrl + latMaxCoord + "&" + longMinUrl + longMinCoord +"&" + longMaxUrl + longMaxCoord;
                    return axios.get(url);
                    // .then(response => {
                    //     console.log("THIS IS MY RESPONSE", response);
                    //     this.numberOfProperties = response.data.result_count;
                    //     console.log("this.numberOfProperties", this.numberOfProperties);
                    // })
                    // .catch(function (error) {
                    //     console.log("THIS IS THE ERROR:", error);
                    // });
                };

                // const getPropertiesFunc = getProperties.bind(this);
                // let axiosGet = getPropertiesFunc(latMin,latMax,longMin,longMax);
             
            
            });

                const axios = require('axios');
                //    let axiosGet = getProperties(latMin,latMax,longMin,longMax);
                axios.get('http://api.zoopla.co.uk/api/v1/property_listings.js?api_key=p6b66f54ehddddjkd7kkm7ec&lat_min=51.479679206083894&lat_max=51.52967920608389&lon_min=-0.21490918675535794&lon_max=-0.16490918675535796')
                .then(response => {
                    console.log("THIS IS MY RESPONSE", response);
                    this.numberOfProperties = response.data.result_count;
                    console.log("this.numberOfProperties", this.numberOfProperties);
                })
                .catch(function (error) {
                    console.log("THIS IS THE ERROR:", error);
                });

            console.log("numberOfProperties in mounted", this.numberOfProperties);
        },

        methods: {
            changeTitle() {
                this.title = this.title.toUpperCase();
                return this.title;
            }
        },

        components: {
            "result-box": result
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
