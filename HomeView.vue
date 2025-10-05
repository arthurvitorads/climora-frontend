<template>
  <v-app style="overflow-x: hidden;">
    <v-app-bar app color="primary" dark flat>
      <v-toolbar-title class="font-weight-bold">CLIMORA</v-toolbar-title>
       <img src="../assets/logo.png" alt="CLIMORA Logo" style="height:32px; margin-right:12px;" />
      <v-spacer></v-spacer>

      <v-btn text href="#about" class="d-none d-md-flex">About</v-btn>
      <v-btn text href="#downloads" class="d-none d-md-flex">Downloads</v-btn>
      <v-btn text href="#video" class="d-none d-md-flex">Vídeo</v-btn>
      <v-btn text href="#link" class="d-none d-md-flex">Link</v-btn>

      <v-app-bar-nav-icon class="d-flex d-md-none" @click="drawer = !drawer"></v-app-bar-nav-icon>
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" app temporary color="primary" location="right" dark
    >
      <v-list>
        <v-list-item link @click="drawer = false" href="#about">
          <v-list-item-title>About</v-list-item-title>
        </v-list-item>
        <v-list-item link @click="drawer = false" href="#downloads">
          <v-list-item-title>Downloads</v-list-item-title>
        </v-list-item>
        <v-list-item link @click="drawer = false" href="#video">
          <v-list-item-title>Video</v-list-item-title>
        </v-list-item>
        <v-list-item link @click="drawer = false" href="#link">
          <v-list-item-title>Link</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <div class="d-sm-flex align-center justify-center">
        <div class="pa-0">
          <img src="../assets/logo.png" max-width="320" class="mx-auto"/>
        </div>
        <div id="about" class="py-16 text-center w-sm-50">
          <h1 class="text-h3 font-weight-bold mb-4">Discover CLIMORA</h1>
          <p class="text-body-1 mb-6">
           CLIMORA combines technology, collaboration, and NASA data to deliver real-time weather forecasts, enhanced by real user reports.
           A new way to understand and share the weather.
          </p>
        </div>
      </div>
      <v-container id="downloads" class="py-16 text-center grey lighten-4">
        <h2 class="text-h4 font-weight-bold mb-4">Download and Explore</h2>
        <p class="text-body-1 mb-6">
          Download the web version, access the previews, or see the app in action.
        </p>
      <v-row
        justify="center"
        class="gap-4"
        style="flex-direction: column; align-items: center;"
      >
          <v-btn color="primary" size="large" class="mb-4" style="width: 300px" href="https://www.canva.com/design/DAG02QquL4w/nZR6OfsOBz_tqnfPEM1mgw/view?utm_content=DAG02QquL4w&utm_campaign=designshare&utm_medium=link2&utm_source=uniquelinks&utlId=h1572a00396"
            target="_blank">
            Canva Slides
          </v-btn>
          <v-btn color="secondary" size="large" class="mb-4" style="width: 300px;">
            Preview Mobile
          </v-btn>
          <v-btn color="success" size="large" style="width: 300px;" href="/canvas.pdf" download>
            Download Canvas
          </v-btn>
      </v-row>
      </v-container>

      <v-container id="video" class="py-16 text-center">
        <h2 class="text-h4 font-weight-bold mb-4">Watch the Pitch Video</h2>
        <div class="d-flex justify-center">
          <iframe width="560" height="315" src="https://www.youtube.com/embed/L_0PwTfDW-A" title="CLIMORA - Pitch" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen>
          </iframe>
        </div>
      </v-container>


      <v-container id="link" class="py-16 text-center grey lighten-4">
        <h2 class="text-h4 font-weight-bold mb-4">Learn more</h2>
        <p class="text-body-1 mb-6">
          Discover more about CLIMORA and how we’re transforming the way people understand the weather.
        </p>
        <v-btn color="primary" size="large" href="https://seusite.com" target="_blank" rounded elevation="4">
          Acess the APP oficial
        </v-btn>
      </v-container>
    </v-main>

    <v-footer color="primary" dark padless>
      <v-col class="text-center py-4">
        © {{ new Date().getFullYear() }} Team BMG — CLIMORA 🚀
      </v-col>
    </v-footer>
  </v-app>
</template>

<script setup lang="ts">
  import { ref } from 'vue';
  const drawer = ref(false)

</script>

<style>

.d-none {
  display: none !important;
}
.d-flex {
  display: flex !important;
}
.d-md-flex {
  display: none !important; 
}

@media (min-width: 960px) {
  .d-md-flex {
    display: flex !important; 
  }
  .d-md-none {
    display: none !important; 
  }
}
</style>