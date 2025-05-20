
<template>
    <div class="flex flex-col items-center justify-center h-screen ">
        <p class="font-bold text-xl">{{ movie.title }}</p>
        <div class="flex items-center m-5 justify-center gap-10">
            <div v-if="movie.image && movie.image.url">
                <img
                    :src="movie.image.url.startsWith('http') ? movie.image.url : `${API_URL.replace('/api','')}${movie.image.url}`"
                    :alt="movie.title"
                    class="max-w-[200px]"
            ></div>
            <div>
                 <div><strong>Réalisateur :</strong> {{ movie.director?.firstname }} {{ movie.director?.lastname }}</div>
                <div v-for="genre in movie.genres" :key="genre.id"><strong>Genre :</strong> {{ genre.name }}</div>
                <div><strong>Année :</strong> {{ movie.year }}</div>
                <div><strong>Durée :</strong> {{ movie.duration }} minutes</div>
                <p><strong>Description :</strong></p>
                <p>{{ movie.description }}</p>
            </div>
        </div>
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


</style>