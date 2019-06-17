<template>
<div class="map-container">
    <div class="google-map" :id="mapName"></div>
    <result-box 
        :numberOfProperties="numberOfProperties" 
        :averagePrice="averagePrice" 
        :showResultBox="showResultBox">
    </result-box>
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
                averagePrice: 0,
                showResultBox: false
            }
        },

        created() {
            this.numberOfProperties = 0;
            this.averagePrice = 0;
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
                const a = 0.025;
                const b = 0.025;
                const latMax = this.createLatitudeMax(lat,a);
                const latMin = this.createLatitudeMin(lat,a); 
                const longMax = this.createLongitudeMax(long,b);
                const longMin = this.createLongitudeMin(long,b);
                const p1 = this.createP1(latMax,long,map);
                const p2 = this.createP2(latMin,long,map);
                const p3 = this.createP3(lat,longMax,map);
                const p4 = this.createP4(lat,longMin,map);

                function getProperties() {
                    const axios = require('axios');
                    let url = this.createUrl(latMin,latMax,longMin,longMax);
                    axios.get(url).then(response => {
                        this.numberOfProperties = response.data.result_count;
                        this.averagePrice = this.getAveragePrice(response.data.listing, this.numberOfProperties);
                        this.showResultBox = true;
                    })
                    .catch(function (error) {
                        console.log("This is an error placeholder, should be removed when proper error handling is implemented", error);
                    });
                };
                const getPropertiesFunc = getProperties.bind(this);
                getPropertiesFunc();
            }.bind(this));
        },

        

        methods: {

            createLatitudeMax(latitude,a) {
               let latMax = latitude + a 
               return latMax
            },
            
            createLatitudeMin(latitude,a) {
               let latMin = latitude - a 
               return latMin
            },

            createLongitudeMax(longitude,b) {
               let longMax = longitude + b 
               return longMax
            },

            createLongitudeMin(longitude,b) {
               let longMin = longitude - b 
               return longMin
            },
      
            createP1(latMax,long,map) {
                const p1 = new google.maps.Marker({
                    position: new google.maps.LatLng(latMax,long),
                    map: map,
                    draggable: false,
                });
                return p1
            },

            createP2(latMin,long,map) {
                 const p2 = new google.maps.Marker({
                    position: new google.maps.LatLng(latMin,long),
                    map: map,
                    draggable: false,
                });
                return p2
            },

            createP3(lat,longMax,map) {
                  const p3 = new google.maps.Marker({
                    position: new google.maps.LatLng(lat,longMax),
                    map: map,
                    draggable: false,
                });
                return p3
            },

            createP4(lat,longMin,map) {
                    const p4 = new google.maps.Marker({
                    position: new google.maps.LatLng(lat,longMin),
                    map: map,
                    draggable: false,
                });
                return p4
            },

            createUrl(latMinCoord,latMaxCoord,longMinCoord,longMaxCoord) {
                let baseURL = "https://api.zoopla.co.uk/api/v1/property_listings.js";
                let apiKey = "api_key=";
                let apiKeyValue = "p6b66f54ehddddjkd7kkm7ec";
                let latMinUrl = "lat_min=";
                let latMaxUrl  = "lat_max=";
                let longMinUrl  = "lon_min=";
                let longMaxUrl  = "lon_max=";
                let url = baseURL + "?" + apiKey + apiKeyValue + "&"+ latMinUrl + latMinCoord + "&" + latMaxUrl + latMaxCoord + "&" + longMinUrl + longMinCoord +"&" + longMaxUrl + longMaxCoord;
                return url
            },

            getAveragePrice(listing, numberOfProperties) {
                let totalPrice = 0;
                for (var i=0; i < listing.length; i++) {
                    totalPrice += parseInt(listing[i].price);
                }
                return totalPrice/numberOfProperties;
            },
 
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
  margin-bottom: 20px;
}
</style>
