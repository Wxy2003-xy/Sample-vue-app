
<template>
  <MapComponent>
 </MapComponent>
</template>

<script lang = 'ts'> 
import MapComponent from './MapComponent.vue';

export default {
    name: "TodoList",
    components: {
        MapComponent,
    },
    data() {

    },
    mounted() {
        console.log("todolist mounted");
    },
    methods: {

    }
}


</script>
