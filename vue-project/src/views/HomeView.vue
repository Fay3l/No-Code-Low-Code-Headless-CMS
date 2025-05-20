<template>
  <div class="home-view">
    <h1>Liste des Films</h1>
    <div v-if="movies.length">
      <ul>
        <li v-for="movie in movies" :key="movie.id">
          <strong>{{ movie.title }}</strong> - 
          <span v-if="movie.genre">{{ movie.genre.name }}</span>
          <span v-if="movie.director">, Réalisé par {{ movie.director.name }}</span>
        </li>
      </ul>
    </div>
    <div v-else>
      <p>Aucun film trouvé.</p>
    </div>

    <h2>Genres</h2>
    <div v-if="genres.length">
      <ul>
        <li v-for="genre in genres" :key="genre.id">{{ genre.name }}</li>
      </ul>
    </div>
    <div v-else>
      <p>Aucun genre trouvé.</p>
    </div>

    <h2>Rechercher un Réalisateur</h2>
    <input v-model="directorSearch" placeholder="Nom du réalisateur" @input="searchDirectors" />
    <div v-if="directors.length">
      <ul>
        <li v-for="director in directors" :key="director.id">{{ director.name }}</li>
      </ul>
    </div>
    <div v-else-if="directorSearch">
      <p>Aucun réalisateur trouvé.</p>
    </div>

    <div v-if="isAuthenticated">
      <h2>Accès Authentifié</h2>
      <p>Vous êtes connecté. Voici toutes les listes :</p>
      <h3>Tous les Films</h3>
      <ul>
        <li v-for="movie in allMovies" :key="movie.id">{{ movie.title }}</li>
      </ul>
      <h3>Tous les Genres</h3>
      <ul>
        <li v-for="genre in allGenres" :key="genre.id">{{ genre.name }}</li>
      </ul>
      <h3>Tous les Réalisateurs</h3>
      <ul>
        <li v-for="director in allDirectors" :key="director.id">{{ director.name }}</li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'
import axios from 'axios'

// Remplacez par l'URL de votre API Strapi
const API_URL = 'http://localhost:1337/api'

// Authentification simulée (remplacez par votre logique réelle)
const isAuthenticated = ref(false) // Passez à true pour tester la partie authentifiée

const movies = ref([])
const genres = ref([])
const directors = ref([])
const directorSearch = ref('')

// Pour la partie authentifiée
const allMovies = ref([])
const allGenres = ref([])
const allDirectors = ref([])

const fetchMovies = async () => {
  const { data } = await axios.get(`${API_URL}/movies?populate=genre,director`)
  movies.value = data.data.map(item => ({
    id: item.id,
    ...item.attributes,
    genre: item.attributes.genre?.data ? { id: item.attributes.genre.data.id, ...item.attributes.genre.data.attributes } : null,
    director: item.attributes.director?.data ? { id: item.attributes.director.data.id, ...item.attributes.director.data.attributes } : null,
  }))
}

const fetchGenres = async () => {
  const { data } = await axios.get(`${API_URL}/genres`)
  genres.value = data.data.map(item => ({
    id: item.id,
    ...item.attributes,
  }))
}

const searchDirectors = async () => {
  if (!directorSearch.value) {
    directors.value = []
    return
  }
  const { data } = await axios.get(`${API_URL}/directors?filters[name][$containsi]=${directorSearch.value}`)
  directors.value = data.data.map(item => ({
    id: item.id,
    ...item.attributes,
  }))
}

// Pour la partie authentifiée
const fetchAllLists = async () => {
  const [moviesRes, genresRes, directorsRes] = await Promise.all([
    axios.get(`${API_URL}/movies`),
    axios.get(`${API_URL}/genres`),
    axios.get(`${API_URL}/directors`)
  ])
  allMovies.value = moviesRes.data.data.map(item => ({ id: item.id, ...item.attributes }))
  allGenres.value = genresRes.data.data.map(item => ({ id: item.id, ...item.attributes }))
  allDirectors.value = directorsRes.data.data.map(item => ({ id: item.id, ...item.attributes }))
}

onMounted(async () => {
  await fetchMovies()
  await fetchGenres()
  if (isAuthenticated.value) {
    await fetchAllLists()
  }
})
</script>

<style scoped>
.home-view {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}
input {
  margin-bottom: 1rem;
  padding: 0.5rem;
  width: 300px;
}
</style>
