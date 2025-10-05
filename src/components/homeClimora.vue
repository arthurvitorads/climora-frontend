<template>
  <v-app class="bg-gradient">
    <v-container class="fill-height d-flex flex-column justify-space-evenly  py-6" fluid>
      <v-navigation-drawer v-model="drawer" app temporary color="white">
        <v-list nav>
          <v-list-item>
            <v-list-item-title class="text-h6 font-weight-bold">Menu</v-list-item-title>
          </v-list-item>
          <v-divider></v-divider>
          <v-list-item v-for="item in menuItems" :key="item.title" link @click="navigate(item.route)">
            <v-list-item-icon>
              <v-icon :icon="item.icon"></v-icon>
            </v-list-item-icon>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>

      <div class="d-flex justify-space-between" style="width: 80vw;" >
        <v-btn icon="mdi-menu" color="blue-darken-3" size="large" class="rounded-circle elevation-4" @click="drawer = !drawer"></v-btn>
        <v-btn icon="mdi-map-marker" color="blue-darken-3" size="large" class="rounded-circle elevation-4"></v-btn>
      </div>

      <div class="text-center mt-6">
        <h2 class="text-h3 font-weight-bold text-blue-lighten-4" style="margin-right: 100px;">Informe</h2>
        <h1 class="text-h3 font-weight-bold text-white">o destino!</h1>
      </div>

      <div class="d-flex justify-center align-center mt-n6">
        <v-text-field style="width: 300px;" placeholder="Pesquisar..." variant="solo-filled" hide-details append-inner-icon="mdi-magnify"/>
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
    </v-container>
  </v-app>
</template>

<script setup lang="ts">
import { ref } from "vue";

  const drawer = ref(false);

  const menuItems = [
    { title: "Início", icon: "mdi-home", route: "/" },
    { title: "Mapa", icon: "mdi-account", route: "/perfil" },
    { title: "Comunidade", icon: "mdi-cog", route: "/configuracoes" },
  ];

  const navigate = (route: any) => {
    console.log("Navegar para:", route);
    drawer.value = false;
  };
</script>

<style scoped>
.bg-gradient {
  background: linear-gradient(to bottom, #1e3a8a, #a78bfa, #e0e7ff);
  min-height: 100vh;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
