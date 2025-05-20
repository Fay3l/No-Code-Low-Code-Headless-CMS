<template>
  <div class="home-view">
    <h1>Liste des Films</h1>
    <div v-if="loading">Chargement...</div>
    <div v-else>
      <div v-for="movie in movies" :key="movie.id" class="movie-card">
        <h2>{{ movie.title }}</h2>
        <p><strong>Réalisateur:</strong> {{ movie.director?.name || 'Inconnu' }}</p>
        <p>
          <strong>Genres:</strong>
          <span v-for="genre in movie.genres" :key="genre.id">
            {{ genre.name }}<span v-if="!isLastGenre(movie.genres, genre)">, </span>
          </span>
        </p>
        <p>{{ movie.description }}</p>
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
    const response = await axios.get(`${API_URL}/films`, {
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
.home-view {
  max-width: 800px;
  margin: auto;
  padding: 2rem;
}
.movie-card {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 1rem;
  margin-bottom: 1.5rem;
}
</style>