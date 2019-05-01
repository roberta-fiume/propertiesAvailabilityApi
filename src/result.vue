<template>
        <div v-if="showResultBoxProp" id="resultBox">
        <!-- <div id="resultBox"> -->
            <h1>the total of properties is: {{numberOfPropertiesProp}}</h1>
            <h1>the average price is: {{averagePriceProp}}</h1>
       </div>
</template>

<script>
export default {
    props: ['numberOfPropertiesProp', 'averagePriceProp', 'showResultBoxProp'],

    data() {
        return {
            
        }
    },
   

    created() {
        this.newProperty;
    },

    methods: {
    }
}
</script>

<style>

#resultBox {
    display: flex;
    width: 500px;
    height: 200px;
    border: 2px solid blue;

    /* position: relative;
  width:40px;
  height:40px;
	left: 50px;
	top: 50px;
  background-color:red;
	transition: left .5s cubic-bezier(.42,-0.3,.78,1.25), 
			top .5s cubic-bezier(.42,-0.3,.78,1.25); */
}

</style>





