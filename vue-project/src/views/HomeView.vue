<template>
  <div class="flex flex-col items-center justify-center" >
    <p class="font-bold text-2xl m-2">Liste des Films</p>
    <div v-if="loading">Chargement...</div>
    <div v-else class="grid grid-cols-4 gap-4 m-5">
      <div v-for="movie in movies" :key="movie.id" class="border-1 border-gray-300 rounded-lg flex flex-col items-center p-4 ">
      <button @click="$router.push(`film/${movie.documentId}`)" style="border: none; background: none; padding: 0;">
        <img
          v-if="movie.image && movie.image.url"
          :src="movie.image.url.startsWith('http') ? movie.image.url : `${API_URL.replace('/api','')}${movie.image.url}`"
          :alt="movie.title"
          style="max-width: 200px; display: block; margin-bottom: 1rem;"
        />
        <p class="font-bold">{{ movie.title }}</p>
        <p><strong>Réalisateur: </strong> {{ movie.director?.firstname +" "+ movie.director?.lastname || 'Inconnu' }}</p>
        <p>
          <strong>Genres: </strong>
          <span v-for="genre in movie.genres" :key="genre.id">
            {{ genre.name }}
          </span>
        </p>
        <p><strong>Année: </strong> {{ movie.year }}</p>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const movies = ref([])
const loading = ref(true)

// Remplacez par votre URL Strapi
const API_URL = 'http://localhost:1337/api'

// Remplacez ceci par la gestion réelle du token après connexion
const token = localStorage.getItem('jwt')



onMounted(async () => {
  loading.value = true
  try {
    const response = await axios.get(`${API_URL}/films?populate=*`, {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    })
    // Strapi v4 retourne les données dans response.data.data
    movies.value = response.data.data
    console.log('Films récupérés:', movies.value)
  } catch (e) {
    console.log('Erreur lors du chargement des films ',e)
  } finally {
    loading.value = false
  }
})
</script>

<style scoped>

</style>