<template>
  <div class="login-page">
    <div class="login-box">
      <h1>Inicio de Sesión</h1>

      <p v-if="errorMessage" class="error">{{ errorMessage }}</p>

      <form @submit.prevent="handleLogin">
        <div class="input-group">
          <label for="username">Usuario:</label>
          <input
            id="username"
            v-model="username"
            type="text"
            placeholder="Ingresa tu usuario"
            required
          />
        </div>

        <div class="input-group">
          <label for="password">Contraseña:</label>
          <input
            id="password"
            v-model="password"
            type="password"
            placeholder="Ingresa tu contraseña"
            required
          />
        </div>

        <button type="submit" class="login-btn">Iniciar Sesión</button>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const username = ref('')
const password = ref('')
const errorMessage = ref('')

const router = useRouter()

async function handleLogin() {
  errorMessage.value = ''

  try {
    const response = await fetch('https://dummyjson.com/auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        username: username.value,
        password: password.value
      })
    })

    const data = await response.json()

    if (!response.ok) {
      errorMessage.value = data.message || 'Error en la autenticación'
      return
    }

    localStorage.setItem('authToken', data.token)
    router.push('/dashboard')
  } catch (err) {
    errorMessage.value = 'Error de conexión o de servidor'
  }
}
</script>

<style scoped>
.login-page {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.login-box {
  background-color: rgba(255, 255, 255, 0.95); 
  padding: 2rem;
  border-radius: 8px;
  width: 360px;
  text-align: center;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
  color: #333;
}

.login-box h1 {
  margin-bottom: 1.5rem;
  font-size: 1.8rem;
  color: #333;
}

.error {
  color: red;
  margin-bottom: 1rem;
}

.input-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 1rem;
  text-align: left;
}

.input-group label {
  font-weight: bold;
  margin-bottom: 0.3rem;
  color: #444;
}

.input-group input {
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 1rem;
}

.input-group input:focus {
  outline: none;
  border-color: #3498db;
  box-shadow: 0 0 4px rgba(52, 152, 219, 0.3);
}

.login-btn {
  background-color: #3498db;
  border: none;
  color: #fff;
  padding: 0.8rem 1.2rem;
  font-size: 1rem;
  cursor: pointer;
  border-radius: 4px;
  transition: background-color 0.3s;
  width: 100%;
}

.login-btn:hover {
  background-color: #2980b9;
}
</style>




