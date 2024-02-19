<template>
  <div id="app">
    <h1>Destinos turísticos</h1>

    <p>
      {{ personNumber }} personas durante {{ daysNumber }} días alojándose en {{ selectedAccommodation }}
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
          <option v-for="accommodation in accommodationTypes" :key="accommodation" :value="accommodation"
            class="select-option">{{ accommodation }}</option>
        </select>
      </p>
    </div>

    <MapDestination />

    <div>
      <p>
        Selecciona un destino:
      <ul>
        <li v-for="destination in sortedDestinations" :key="destination.id"
          :class="{ 'expensive': destination.economic_level === 'expensive', 'moderate': destination.economic_level === 'moderate', 'cheap': destination.economic_level === 'cheap' }"
          @click="selectDestination(destination)">
          {{ destination.name }}</li>
      </ul>
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch, defineProps } from 'vue';
import Presupuesto from './components/Presupuesto.vue';
import MapDestination from './components/MapDestination.vue';

const api = "http://localhost:3000/api/destinations";
const personNumber = ref(2);
const daysNumber = ref(7);
const destinations = ref([]);
const sortedDestinations = ref([]);

const costsAccommodations = {
  "Albergue": 25,
  "Hostal": 40,
  "Bed and Breakfast": 65,
  "Hotel 3*": 100,
  "Hotel Superior": 200,
};

const costsDestinations = {
  cheap: 0.7,
  moderate: 1,
  expensive: 1.3
};

const accommodationTypes = ["Albergue", "Hostal", "Bed and Breakfast", "Hotel 3*", "Hotel Superior"];
const selectedAccommodation = ref('Hostal');
const selectedDestination = ref(null);
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
  const response = await fetch(api);
  return response.json();
};

const sortDestinations = () => {
  sortedDestinations.value = destinations.value.sort((a, b) => a.name.localeCompare(b.name));
};

const selectDestination = (destination) => {
  selectedDestination.value = destination;
};

const calculateEstimation = () => {
  if (selectedDestination.value && selectedAccommodation.value) {
    const destinationCost = costsDestinations[selectedDestination.value.price];
    const accommodationCost = costsAccommodations[selectedAccommodation.value];
    return (personNumber.value * daysNumber.value * destinationCost * accommodationCost).toFixed(2);
  } else {
    return 0;
  }
};

watch([personNumber, daysNumber, selectedDestination, selectedAccommodation], () => {
  estimatedBudget.value = calculateEstimation();
});

</script>

<style scoped>
#persons {
  background-color: beige;
}

#days {
  background-color: beige;
}

.expensive {
  color: red;
}

.moderate {
  color: orange;
}

.cheap {
  color: green;
}
</style>