<template>
  <v-app class="bg-gradient">
    <v-container class="fill-height d-flex flex-column justify-space-evenly py-6" fluid>
      <v-navigation-drawer v-model="drawer" app temporary color="white">
        <v-list nav>
          <v-list-item>
            <v-list-item-title class="text-h6 font-weight-bold">Menu</v-list-item-title>
          </v-list-item>
          <v-divider></v-divider>
          <v-list-item v-for="item in menuItems" :key="item.title">
            <v-list-item-icon>
              <v-icon :icon="item.icon"></v-icon>
            </v-list-item-icon>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>

      <div class="d-flex justify-space-between" style="width: 80vw;">
        <v-btn icon="mdi-menu" color="blue-darken-3" size="large" class="rounded-circle elevation-4" @click="drawer = !drawer"></v-btn>
        <v-btn icon="mdi-map-marker" color="blue-darken-3" size="large" class="rounded-circle elevation-4"></v-btn>
      </div>

      <div class="text-center mt-6">
        <h2 class="text-h3 font-weight-bold text-blue-lighten-4" style="margin-right: 100px;">Informe</h2>
        <h1 class="text-h3 font-weight-bold text-white">o destino!</h1>
      </div>

      <div class="d-flex justify-center align-center mt-n6">
        <v-text-field style="width: 300px;" placeholder="Pesquisar..." variant="solo-filled" hide-details append-inner-icon="mdi-magnify" readonly @focus="openSearch"/>
      </div>

      <v-card class="mt-8 pa-4 rounded-xl" elevation="2" color="white">
        <div class="d-flex align-center mb-3">
          <v-icon color="blue" class="mr-2">mdi-account-group</v-icon>
          <h3 class="text-subtitle-1 font-weight-bold">Comunidade</h3>
        </div>
        <v-divider class="mb-3" />
        <v-list density="comfortable">
          <v-list-item v-for="n in 2" :key="n" class="mb-2 rounded-lg" color="grey-lighten-4">
            <template #prepend>
              <v-avatar color="grey-lighten-2">
                <v-icon>mdi-account</v-icon>
              </v-avatar>
            </template>
            <v-list-item-title class="font-weight-medium">Fulano da Silva</v-list-item-title>
            <v-list-item-subtitle>Trilha das perobas está infestada de cobras</v-list-item-subtitle>
          </v-list-item>
        </v-list>
      </v-card>

      <v-card v-if="weatherDetails && weatherDetails.length" class="mt-8 pa-4 rounded-xl" elevation="2" color="white">
        <div class="d-flex align-center mb-3">
          <v-icon color="blue" class="mr-2">mdi-weather-partly-cloudy</v-icon>
          <h3 class="text-subtitle-1 font-weight-bold">Clima em {{ selectedCity }}</h3>
        </div>
        <v-divider class="mb-3" />
        <v-simple-table>
          <thead>
            <tr>
              <th>Data</th>
              <th>Temperatura (°C)</th>
              <th>Precipitação (mm)</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="day in weatherDetails" :key="day.date">
              <td>{{ day.date }}</td>
              <td>{{ day.temp }}</td>
              <td>{{ day.precip }}</td>
            </tr>
          </tbody>
        </v-simple-table>
      </v-card>
      <div v-if="loadingWeather" class="text-center mt-4">Carregando clima...</div>
    </v-container>

    <v-overlay :model-value="searchActive" @update:model-value="val => searchActive = val" absolute opacity="0.95">
      <v-card class="pa-4" elevation="4" style="width: 100vw; height: 100vh; border-radius: 0;">
        <v-text-field v-model="searchQuery" placeholder="Pesquisar..." append-inner-icon="mdi-magnify" outlined dense hide-details/>
        <v-list dense>
          <v-list-item v-for="(city, index) in filteredCities" :key="index" class="mb-2 rounded-lg" color="grey-lighten-3">
            <v-btn block color="blue-lighten-4" class="text-black" @click="selectCity(city)">
              <v-list-item-icon>
                <v-icon>mdi-map-marker</v-icon>
              </v-list-item-icon>
              <v-list-item-title>{{ city }}</v-list-item-title>
            </v-btn>
          </v-list-item>
        </v-list>
        <v-btn block color="grey lighten-1" class="mt-4" @click="searchActive = false">
          <v-icon left>mdi-arrow-left</v-icon> Voltar
        </v-btn>
      </v-card>
    </v-overlay>

  </v-app>
</template>

<script setup lang="ts">

import { ref, computed, watch } from 'vue'

const drawer = ref(false)
const searchActive = ref(false)
const searchQuery = ref("")

const menuItems = [
  { title: "Início", icon: "mdi-home", route: "/" },
  { title: "Mapa", icon: "mdi-account", route: "/perfil" },
  { title: "Comunidade", icon: "mdi-cog", route: "/configuracoes" },
]

const cities = ref(["São Paulo", "Rio de Janeiro", "Brasília"])

const filteredCities = computed(() =>
  cities.value.filter(c => c.toLowerCase().includes(searchQuery.value.toLowerCase()))
)


const selectedCity = ref<string | null>(null)
const weatherDetails = ref<any[]>([])
const loadingWeather = ref(false)

const coords: Record<string, { lat: number, lon: number }> = {
  "São Paulo": { lat: -23.55, lon: -46.63 },
  "Rio de Janeiro": { lat: -22.91, lon: -43.17 },
  "Brasília": { lat: -15.78, lon: -47.93 },
}

async function fetchWeather(city: string) {
  loadingWeather.value = true;
  weatherDetails.value = [];
  const coord = coords[city] ?? coords["São Paulo"] ?? { lat: -23.55, lon: -46.63 };
  const lat = coord.lat;
  const lon = coord.lon;
  const today = new Date();
  const endDate = new Date(today);
  endDate.setDate(endDate.getDate() - 3);
  const startDate = new Date(today);
  startDate.setDate(startDate.getDate() - 4);
  const formatDate = (date: Date) => (date instanceof Date && !isNaN(date.getTime())) ? date.toISOString().split("T")[0].replace(/-/g, "") : "";
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
    weatherDetails.value = days;
  } catch (e) {
    weatherDetails.value = [];
    // opcional: mostrar erro
  } finally {
    loadingWeather.value = false;
  }
}



const selectCity = (city: string) => {
  selectedCity.value = city;
  fetchWeather(city);
}

const openSearch = () => {
  searchActive.value = true
}
</script>

<style scoped>
.bg-gradient {
  background: linear-gradient(to bottom, #1e3a8a, #a78bfa, #e0e7ff);
  min-height: 100vh;
  background-repeat: no-repeat;
  background-size: cover;
}
.v-overlay__content {
  display: flex;
  justify-content: center;
  align-items: center;

}
</style>