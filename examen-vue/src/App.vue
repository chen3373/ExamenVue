<template>
  <div id="app">
    <h1>Destinos turísticos</h1>

    <p>
      {{ personNumber }} personas durante {{ daysNumber }} dias alojandose en {{ selectedAccommodation }}
    </p>

    <p style="font-weight: bold;">
      Se estima {{ estimatedBudget }}€
    </p>

    <Presupuesto />

    <div>
      <p id="persons">
        Número de personas:
        <input type="number" min="1" max="10" v-model="personNumber">
        <input type="range" min="1" max="10" v-model="personNumber">
      </p>
      <p id="days">
        Número de días:
        <input type="number" min="1" max="30" v-model="daysNumber">
        <input type="range" min="1" max="10" v-model="daysNumber">
      </p>
    </div>

    <div id="accommodation">
      <p>
        Tipo de alojamiento: 
        <select v-model="selectedAccommodation">
          <option v-for="accommodation in accommodationTypes" :key="accommodation" :value="accommodation" class="select-option">{{ accommodation }}</option>
        </select>
      </p>
    </div>

    <div>
      <p>
        Selecciona un destino: 
        <ul>
          <li v-for="destination in sortedDestinations" :key="destination.id">{{ destination.name }}</li>
        </ul>
      </p>
    </div>

  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import Presupuesto from './components/Presupuesto.vue';

const api = "http://localhost:3000/api/";
const personNumber = ref(2);
const daysNumber = ref(7);
const destinations = ref([]);
const sortedDestinations = ref([]);

const accommodationTypes = ["Albergue", "Hostal", "Bed and Breakfast", "Hotel 3*", "Hotel Superior" ];
const selectedAccommodation = ref('Hostal');
const estimatedBudget = ref(0);

const fetchDestinations = async () => {
  try {
    const data = await getDestinations();
    destinations.value = data;
    sortDestinations();
  } catch (error) {
    console.error('Error al obtener los destinos:', error);
  }
};

onMounted(fetchDestinations);

const getDestinations = async () => {
  const response = await fetch(`${api}destinations`);
  return response.json();
};

const sortDestinations = () => {
  sortedDestinations.value = destinations.value.sort((a, b) => a.name.localeCompare(b.name));
};
</script>

<style scoped>

#persons {
  background-color: beige;
}

#days {
  background-color: beige;
}

</style>


