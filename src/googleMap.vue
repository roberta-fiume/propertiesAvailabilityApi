<template>
    <div class="google-map" :id="mapName"></div>
</template>

<script>
    export default {
        name: 'google-map',

        props: ['name'],

        data() {
            return {
                mapName: this.name + "-map",
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
                const lat = event.qa.y;
                const long = event.qa.x;
                const coordinates = getCoordinates(lat, long);
                console.log("THESE ARE MY COORDINATES:", coordinates)
            })
            
            function getCoordinates(latY, longX) {
                const coordinates = {"latitude": latY, "longitude": longX}
                return coordinates
            }

            function createBoundingBox(latMin, latMax, longMin, longMax) {
                const boundingCoordinates = {"latitudeMin": latMin, "latitudeMax": latMax, "longitudeMin": longMin, "longitudeMax": longMax} 
                return boundingCoordinates
            }
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
