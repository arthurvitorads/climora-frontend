<template>
  <v-container class="py-10 text-center">
    <h2 class="text-h4 font-weight-bold mb-4">Clima via NASA POWER API</h2>

    <v-text-field
      v-model="city"
      label="Cidade"
      outlined
      class="mb-4"
    ></v-text-field>
    <v-btn color="primary" @click="getWeather">Buscar Clima</v-btn>

    <div v-if="loading" class="mt-4">Carregando...</div>

    <div v-if="weather && weather.length" class="mt-6">
      <h3 class="text-h6">Clima em {{ city }}:</h3>

      <v-simple-table>
        <thead>
          <tr>
            <th>Data</th>
            <th>Temperatura (°C)</th>
            <th>Precipitação (mm)</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="day in weather" :key="day.date">
            <td>{{ day.date }}</td>
            <td>{{ day.temp }}</td>
            <td>{{ day.precip }}</td>
          </tr>
        </tbody>
      </v-simple-table>
    </div>
  </v-container>
</template>

<script setup>
import { ref } from "vue";

const city = ref("São Paulo");
const weather = ref([]);
const loading = ref(false);

const coords = {
  "São Paulo": { lat: -23.55, lon: -46.63 },
  "Rio de Janeiro": { lat: -22.91, lon: -43.17 },
  "Brasília": { lat: -15.78, lon: -47.93 },
};

async function getWeather() {
  loading.value = true;
  weather.value = [];

  const { lat, lon } = coords[city.value] || coords["São Paulo"];

  // ontem e anteontem
  const today = new Date();
  const endDate = new Date(today);
  endDate.setDate(endDate.getDate() - 3);
  const startDate = new Date(today);
  startDate.setDate(startDate.getDate() - 4);

  const formatDate = (date) => date.toISOString().split("T")[0].replace(/-/g, "");
  const start = formatDate(startDate);
  const end = formatDate(endDate);

  const url = `https://power.larc.nasa.gov/api/temporal/daily/point?parameters=T2M,PRECTOTCORR&start=${start}&end=${end}&latitude=${lat}&longitude=${lon}&format=JSON&community=RE`;

  try {
    const res = await fetch(url);
    const data = await res.json();

    const t2m = data.properties.parameter.T2M;
    const prectot = data.properties.parameter.PRECTOTCORR;

    const tempUnit = data.parameters.T2M.units;
    const precipUnit = data.parameters.PRECTOTCORR.units;

    const days = Object.keys(t2m).map(date => ({
      date,
      temp: t2m[date] === -999.0 ? "Sem dados" : `${t2m[date].toFixed(1)} ${tempUnit}`,
      precip: prectot[date] === -999.0 ? "Sem dados" : `${prectot[date].toFixed(2)} ${precipUnit}`,
    }));

    weather.value = days;

  } catch (e) {
    console.error("Erro ao buscar dados da NASA:", e);
  } finally {
    loading.value = false;
  }
}
</script>
