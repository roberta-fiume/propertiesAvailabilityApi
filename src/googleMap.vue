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
                    console.log("listener added to map after clicked and this is my event", event);
                    console.log("my latitude is", event.qa.x);
                    console.log("my longitude is", event.qa.y);
            });
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
