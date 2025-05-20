<template>
<div class="min-h-screen flex items-center justify-center py-8 px-2">
  <div class="w-full max-w-md bg-white rounded-2xl shadow-xl p-8 flex flex-col items-center">
    <div class="mb-6 flex flex-col items-center">
      <svg class="w-14 h-14 text-blue-500 mb-2" fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M15.75 6a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0zM4.5 19.5a7.5 7.5 0 0115 0v.75a.75.75 0 01-.75.75h-13.5a.75.75 0 01-.75-.75V19.5z" /></svg>
      <h2 class="text-2xl font-bold text-gray-800">Créer un compte</h2>
      <p class="text-gray-500 text-sm">Rejoignez-nous pour accéder à la plateforme</p>
    </div>
    <form @submit.prevent="handleSignUp" class="w-full space-y-4">
      <div>
        <label for="username" class="block text-sm font-medium text-gray-700">Nom d'utilisateur</label>
        <input v-model="form.username" id="username" required autocomplete="username" class="mt-1 block w-full rounded-lg border border-gray-300 focus:border-blue-500 focus:ring-2 focus:ring-blue-200 px-3 py-2 text-gray-800 bg-gray-50" />
      </div>
      <div>
        <label for="email" class="block text-sm font-medium text-gray-700">Email</label>
        <input v-model="form.email" id="email" type="email" required autocomplete="email" class="mt-1 block w-full rounded-lg border border-gray-300 focus:border-blue-500 focus:ring-2 focus:ring-blue-200 px-3 py-2 text-gray-800 bg-gray-50" />
      </div>
      <div>
        <label for="password" class="block text-sm font-medium text-gray-700">Mot de passe</label>
        <input v-model="form.password" id="password" type="password" required autocomplete="new-password" class="mt-1 block w-full rounded-lg border border-gray-300 focus:border-blue-500 focus:ring-2 focus:ring-blue-200 px-3 py-2 text-gray-800 bg-gray-50" />
      </div>
      <button type="submit" :disabled="loading" class="w-full flex justify-center items-center gap-2 py-2 px-4 rounded-lg bg-blue-500 hover:bg-blue-600 text-white font-semibold transition disabled:bg-blue-300 disabled:cursor-not-allowed">
        <svg v-if="loading" class="animate-spin h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8v8z"></path></svg>
        <span>{{ loading ? 'Création...' : "S'inscrire" }}</span>
      </button>
      <p v-if="error" class="text-red-500 text-center text-sm mt-2">{{ error }}</p>
      <p v-if="success" class="text-green-600 text-center text-sm mt-2">Compte créé avec succès !</p>
    </form>
    <div class="mt-6 text-sm text-gray-500 text-center">
      Déjà un compte ?
      <router-link to="/login" class="text-blue-500 hover:underline font-medium">Se connecter</router-link>
    </div>
  </div>
</div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import router from '@/router'

const form = ref({
    username: '',
    email: '',
    password: ''
})
const loading = ref(false)
const error = ref('')
const success = ref(false)

const handleSignUp = async () => {
    loading.value = true
    error.value = ''
    success.value = false
    try {
        await axios.post('http://localhost:1337/api/auth/local/register', {
            username: form.value.username,
            email: form.value.email,
            password: form.value.password
        })
        success.value = true
        form.value = { username: '', email: '', password: '' }
        router.push('/login')
    } catch (e) {
        error.value = e.response?.data?.error?.message || 'Erreur lors de la création du compte.'
    } finally {
        loading.value = false
    }
}
</script>

<style scoped>

</style>