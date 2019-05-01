<template>
        <div id="child">
            <h1>the total of properties is: {{numberOfPropertiesProp}}</h1>
       </div>
</template>

<script>
export default {
    props: ['numberOfPropertiesProp'],

    data() {
        return {
            
        }
    },
    // computed: {
    //     newProperty() {
    //      this.numberOfPropertiesProp = 0;
      
    //     }
    // },
    watch: {
        numberOfPropertiesProp(val) {
        //console.log('The property has changed!', this.numberOfPropertiesProp);
        this.numberOfPropertiesProp = val;
        console.log("I AM WATCHED:", this.numberOfPropertiesProp);
        return this.numberOfPropertiesProp;
        },
    },
   

    created() {
        this.newProperty;
        // this.forceRerender();
        console.log("I AM THE CHILD CREATED:", this.numberOfPropertiesProp)
    },

    methods: {
        // forceRerender() {
        //     console.log("HELLOOOO i WORKKKKK")
        //     this.numberOfPropertiesProp = 0;  
        // }
    }
}
</script>

<style>

#child {
    display: flex;
    width: 500px;
    height: 70px;
    border: 2px solid blue;
}

</style>





