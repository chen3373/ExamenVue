<template >
    <div id="app">        
        <p>
            Indica tu Presupuesto
            <input type="number" v-model="presupuesto">
        </p>
        <p v-if="presupuesto == 0">
            Debe indicar un presupuesto mayor a 0
        </p>

        <p v-if="presupuesto > 0">
            <h2>Su Presupuesto es {{ presupuesto }}</h2>
        </p>
        
    </div>
</template>

<script setup>
import { ref } from 'vue';

const presupuesto = ref(0);

</script>
    
<style scoped>
    #app {
        background-color: aquamarine;
    }
</style>