<template>
    <div class="login-container">
        <h2>Connexion</h2>
        <form @submit.prevent="login">
            <input v-model="identifier" type="text" placeholder="Email ou nom d'utilisateur" required />
            <input v-model="password" type="password" placeholder="Mot de passe" required />
            <button type="submit" :disabled="loading">Se connecter</button>
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
.login-container {
    max-width: 400px;
    margin: 50px auto;
    padding: 2rem;
    border: 1px solid #eee;
    border-radius: 8px;
}
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