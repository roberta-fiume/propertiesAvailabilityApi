<template>
<div class="map-container">
    <div class="google-map" :id="mapName"></div>
    <result-box 
        :numberOfPropertiesProp="numberOfProperties" 
        :averagePriceProp="averagePrice" 
        :showResultBoxProp="showResultBox">
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

        watch: {
            numberOfProperties: function(val) {
                this.numberOfProperties = val;
                return this.numberOfProperties;
            },
              averagePrice: function(val) {
                this.averagePrice = val;
                return this.averagePrice;
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
                    const axios = require('axios');
                    let baseURL = "http://api.zoopla.co.uk/api/v1/property_listings.js";
                    let apiKey = "api_key=";
                    let apiKeyValue = "p6b66f54ehddddjkd7kkm7ec";
                    let latMinUrl = "lat_min=";
                    let latMaxUrl  = "lat_max=";
                    let longMinUrl  = "lon_min=";
                    let longMaxUrl  = "lon_max=";
                    let url = baseURL + "?" + apiKey + apiKeyValue + "&"+ latMinUrl + latMinCoord + "&" + latMaxUrl + latMaxCoord + "&" + longMinUrl + longMinCoord +"&" + longMaxUrl + longMaxCoord;
                    axios.get(url).then(response => {
                        this.numberOfProperties = response.data.result_count;
                        this.averagePrice = this.getAveragePrice(response.data.listing, this.numberOfProperties);
                        this.showResultBox = true;
                    })
                    .catch(function (error) {
                        console.log("THIS IS THE ERROR:", error);
                    });
                };

                const getPropertiesFunc = getProperties.bind(this);
                getPropertiesFunc(latMin,latMax,longMin,longMax);
            }.bind(this));
        },

        methods: {
            getAveragePrice(listing, numberOfProperties) {
                let totalPrice = 0;
                for (var i=0; i < listing.length; i++) {
                    totalPrice += parseInt(listing[i].price);
                }
                return totalPrice/numberOfProperties;
            },
            // moveResultBoxToClick() {
            //     var theThing = document.getElementById("resultBox");
            //     var container = document.querySelector(".google-map");
            //     var xPosition = event.clientX - container.getBoundingClientRect().left - (theThing.clientWidth / 2);
            //     var yPosition = event.clientY - container.getBoundingClientRect().top - (theThing.clientHeight / 2);
            //     theThing.style.left = xPosition + "px";
            //     theThing.style.top = yPosition + "px";
            // }
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

  /* position: relative;
	left:100px;
	width: 500px;
	height: 300px;
	border: 1px black solid;
	overflow: hidden;
	background-color: #EEE;
	cursor: pointer; */
}
</style>
