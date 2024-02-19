<template >
    <div id="app">        
        <p>
            Indica tu Presupuesto
            <input type="number" v-model="budget">
        </p>
        <p v-if="budget === 0">
            Debe indicar un presupuesto mayor a 0€
        </p>

        <p v-if="budget > 0">
            <h2>Su Presupuesto es {{ budget }}€</h2>
        </p>
        
    </div>
</template>

<script setup>
import { ref } from 'vue';

const budget = ref(0);

const loadBudget = () => {
  const savedBudget = localStorage.getItem('budget');
  if (savedBudget) {
    budget.value = JSON.parse(savedBudget);
  }
};
</script>
    
<style scoped>
    #app {
        background-color: aquamarine;
    }
</style>