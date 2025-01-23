
<template>
  <div class="container d-flex align-items-center justify-content-center min-vh-100">
    <div class="row w-100 justify-content-center">
      <div class="col-md-6 col-lg-4">
        <div class="login-container p-4">

          <h1 class="text-center mb-4">Inicio de Sesión</h1>
          <p v-if="errorMessage" class="text-danger text-center">{{ errorMessage }}</p>

          <form @submit.prevent="handleLogin">
            <div class="mb-3">
              <label for="username" class="form-label">Usuario</label>
              <input
                id="username"
                v-model="username"
                type="text"
                class="form-control"
                placeholder="Ingresa tu usuario"
                required
              />
            </div>

            <div class="mb-3">
              <label for="password" class="form-label">Contraseña</label>
              <input
                id="password"
                v-model="password"
                type="password"
                class="form-control"
                placeholder="Ingresa tu contraseña"
                required
              />
            </div>

            <button type="submit" class="btn btn-primary w-100">
              Iniciar Sesión
            </button>
          </form>
        </div>
      </div>
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
    console.error(err)
    errorMessage.value = 'Error de conexión o de servidor'
  }
}

</script>

<style scoped>
.login-container {
  border: 1px solid #ddd;
  background-color: #fafafa;
  border-radius: 6px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  color: #333;
}
</style>