<template>
    <div class="movie-details">
        <h1>{{ movie.title }}</h1>
        <ul>
            <li v-if="movie.image && movie.image.url">
                <img
                    :src="movie.image.url.startsWith('http') ? movie.image.url : `${API_URL.replace('/api','')}${movie.image.url}`"
                    :alt="movie.title"
                    style="max-width: 200px; display: block; margin-bottom: 1rem;"
            ></li>
            <li><strong>Réalisateur :</strong> {{ movie.director?.firstname }} {{ movie.director?.lastname }}</li>
            <li v-for="genre in movie.genres" :key="genre.id"><strong>Genre :</strong> {{ genre.name }}</li>
            <li><strong>Année :</strong> {{ movie.year }}</li>
            <li><strong>Durée :</strong> {{ movie.duration }} minutes</li>
        </ul>
        <p><strong>Description :</strong></p>
        <p>{{ movie.description }}</p>
    </div>
</template>

<script setup >
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { useRoute } from 'vue-router'

const movie = ref([])
const loading = ref(true)

// Remplacez par votre URL Strapi
const API_URL = 'http://localhost:1337/api'

// Remplacez ceci par la gestion réelle du token après connexion
const token = localStorage.getItem('jwt')

const route = useRoute()
const documentId = route.params.documentId

onMounted(async () => {
  loading.value = true
  try {
    const response = await axios.get(`${API_URL}/films/${documentId}?populate=*`, {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    })
    // Strapi v4 retourne les données dans response.data.data
    movie.value = response.data.data
    console.log('Document', documentId)
    console.log('Film récupérés:', movie.value)
  } catch (e) {
    console.log('Erreur lors du chargement des films ',e)
  } finally {
    loading.value = false
  }
})
</script>

<style scoped>
.movie-details {
    max-width: 600px;
    margin: 2rem auto;
    padding: 2rem;
    background: #f9f9f9;
    border-radius: 8px;
}
.movie-details h1 {
    margin-bottom: 1rem;
}
.movie-details ul {
    list-style: none;
    padding: 0;
}
.movie-details li {
    margin-bottom: 0.5rem;
}
</style>