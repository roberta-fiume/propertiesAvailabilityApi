<template>
        <div v-if="showResultBox" id="resultBox">
            <h1> {{numberOfProperties}} Properties </h1>
            <h1>Average price is: £{{averagePrice}}</h1>     
       </div>
</template>

<script>
export default {
    props: ['numberOfProperties', 'averagePrice', 'showResultBox'],

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





