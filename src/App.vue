<template>
  <div class="flex justify-center m-2">
    <select v-model="seleccion" class="mb-4 p-2" @change="cambiarSeleccion">
      <option value="recomendaciones">Recomendaciones animes</option>
      <option value="poblacionPaises">Poblacion paises</option>
      <option value="randomAnime">Random Anime</option>
      <option value="randomManga">Random Manga</option>
    </select>
  </div>

    <div v-if="seleccion === 'recomendaciones'" class="flex justify-center flex-wrap gap-4">
    <li v-for="recomendacion in recomendaciones" :key="recomendacion.mal_id" class="list-none">
      <ul>
        <li v-for="anime in recomendacion.entry" :key="anime.mal_id" class="flex flex-col items-center bg-white p-4 rounded-lg shadow-md">
          <h2 class="text-lg font-semibold mb-2">{{ anime.title }}</h2>
          <img :src="anime.images.jpg.image_url" class="w-40 h-60 object-cover rounded-md mb-2" />
        </li>
      </ul>
    </li>
  </div>

  <div v-if="seleccion === 'randomAnime'" class="flex justify-center items-center flex-col bg-white p-4 rounded-lg shadow-md">
    <h2 class="text-lg font-semibold mb-2">{{ anime.title}}</h2>
    <img :src="anime.images.jpg.image_url" class="w-40 h-60 object-cover rounded-md mb-4" />
  </div>

  <div v-if="seleccion === 'randomManga'" class="flex justify-center items-center flex-col bg-white p-4 rounded-lg shadow-md">
    <h2 class="text-lg font-semibold mb-2">{{ manga.title }}</h2>
    <img :src="manga.images.jpg.image_url" class="w-40 h-60 object-cover rounded-md mb-4" />
  </div>
  
</template>

<script setup>
import axios from 'axios'
import { ref } from 'vue'

const seleccion = ref('')
const recomendaciones = ref([])
const manga = ref()
const anime = ref()

const cambiarSeleccion = () => {
  if (seleccion.value === 'recomendaciones') {
    obtenerRecomendaciones();
  } else if (seleccion.value === 'randomAnime') {
    obtenerRandomAnime();
  } else if (seleccion.value === 'randomManga') { 
    obtenerRandomManga();
  }
}

const obtenerRecomendaciones = () => {
  axios.get('https://api.jikan.moe/v4/recommendations/anime')
    .then((response) => {
      recomendaciones.value = response.data.data;
      console.log(recomendaciones.value);
    })
    .catch((err) => {
      console.error(err);
    });
}
const obtenerRandomAnime = () => {
  axios.get('https://api.jikan.moe/v4/random/anime')
    .then((response) => {
      anime.value = response.data.data;
      console.log(recomendaciones.value);
    })
    .catch((err) => {
      console.error(err);
    });
}

const obtenerRandomManga = () => {
  axios.get('https://api.jikan.moe/v4/random/manga')
    .then((response) => {
      manga.value = response.data.data;
      console.log(recomendaciones.value);
    })
    .catch((err) => {
      console.error(err);
    });
}
</script>
