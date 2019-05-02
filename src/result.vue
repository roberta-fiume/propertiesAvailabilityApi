<template>
        <div v-if="showResultBoxProp" id="resultBox">
            <h1> {{numberOfPropertiesProp}} Properties </h1>
            <h1>Average price is: £{{averagePriceProp}}</h1>     
       </div>
</template>

<script>
export default {
    props: ['numberOfPropertiesProp', 'averagePriceProp', 'showResultBoxProp'],

    data() {
        return {
            
        }
    }
}
</script>

<style>

#resultBox {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 600px;
    height: 200px;
    border: 9px solid #42a7f4;
    margin-left: auto;
    margin-right: auto;
    color: #42a7f4;
}

</style>





