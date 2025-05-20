<template> 
    <div class="max-w-[400px] mx-auto mt-[50px] p-8 border border-gray-200 rounded-lg bg-white shadow">
        <p class="block mb-2 text-sm font-medium text-gray-700">Connexion</p>
        <form @submit.prevent="login">
            <!-- Modern input design with Tailwind CSS -->
            <label class="block mb-2 text-sm font-medium text-gray-700">Identifiant</label>
            <input
                v-model="identifier"
                type="text"
                placeholder="Email ou nom d'utilisateur"
                required
                class="mb-4 block w-full rounded-lg border border-gray-300 bg-gray-50 p-2.5 text-gray-900 focus:border-blue-500 focus:ring-blue-500"
            />
            <label class="block mb-2 text-sm font-medium text-gray-700">Mot de passe</label>
            <input
                v-model="password"
                type="password"
                placeholder="Mot de passe"
                required
                class="mb-4 block w-full rounded-lg border border-gray-300 bg-gray-50 p-2.5 text-gray-900 focus:border-blue-500 focus:ring-blue-500"
            />
            <button
                type="submit"
                :disabled="loading"
                class="w-full flex justify-center items-center px-4 py-2 mb-4 bg-blue-600 hover:bg-blue-700 text-white font-semibold rounded-lg transition-colors duration-200 disabled:opacity-50 disabled:cursor-not-allowed"
            >
                <span v-if="loading" class="animate-spin mr-2 h-5 w-5 border-2 border-white border-t-transparent rounded-full"></span>
                Se connecter
            </button>
            <p v-if="error" class="error">{{ error }}</p>
        </form>
    </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import { useRouter } from 'vue-router'

const identifier = ref('')
const password = ref('')
const error = ref('')
const loading = ref(false)
const router = useRouter()

const login = async () => {
    error.value = ''
    loading.value = true
    try {
        const res = await axios.post('http://localhost:1337/api/auth/local', {
            identifier: identifier.value,
            password: password.value,
        })
        // Stockez le JWT dans le localStorage ou Vuex
        localStorage.setItem('jwt', res.data.jwt)
        console.log('Connecté JWT:', res.data.jwt)
        router.push('/')
    } catch (e) {
        error.value = e.response?.data?.error?.message || 'Erreur de connexion'
    } finally {
        loading.value = false
    }
}
</script>

<style scoped>

input, button {
    display: block;
    width: 100%;
    margin-bottom: 1rem;
    padding: 0.5rem;
}
.error {
    color: red;
}
</style>