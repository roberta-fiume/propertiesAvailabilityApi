<template>
 <div>
   <google-map name="example"></google-map>
 </div>
</template>

<script>
 import googleMap from './googleMap.vue'

  export default {
  
    name: 'App',
    
    components: {
      "google-map": googleMap
    }



  
  }


</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
