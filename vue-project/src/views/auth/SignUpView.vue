<template>
    <div class="signup-container">
        <h2>Créer un compte</h2>
        <form @submit.prevent="handleSignUp">
            <div>
                <label for="username">Nom d'utilisateur</label>
                <input v-model="form.username" id="username" required />
            </div>
            <div>
                <label for="email">Email</label>
                <input v-model="form.email" id="email" type="email" required />
            </div>
            <div>
                <label for="password">Mot de passe</label>
                <input v-model="form.password" id="password" type="password" required />
            </div>
            <button type="submit" :disabled="loading">
                {{ loading ? 'Création...' : 'S\'inscrire' }}
            </button>
            <p v-if="error" class="error">{{ error }}</p>
            <p v-if="success" class="success">Compte créé avec succès !</p>
        </form>
    </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

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
    } catch (e) {
        error.value = e.response?.data?.error?.message || 'Erreur lors de la création du compte.'
    } finally {
        loading.value = false
    }
}
</script>

<style scoped>
.signup-container {
    max-width: 400px;
    margin: 40px auto;
    padding: 2rem;
    border: 1px solid #eee;
    border-radius: 8px;
    background: #fff;
}
form > div {
    margin-bottom: 1rem;
}
label {
    display: block;
    margin-bottom: 0.3rem;
}
input {
    width: 100%;
    padding: 0.5rem;
    box-sizing: border-box;
}
button {
    width: 100%;
    padding: 0.7rem;
    background: #42b983;
    color: #fff;
    border: none;
    border-radius: 4px;
    font-weight: bold;
    cursor: pointer;
}
button:disabled {
    background: #aaa;
    cursor: not-allowed;
}
.error {
    color: #e74c3c;
    margin-top: 1rem;
}
.success {
    color: #27ae60;
    margin-top: 1rem;
}
</style>