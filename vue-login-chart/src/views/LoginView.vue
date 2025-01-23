<!-- LoginView.vue -->
<template>
  <div class="container d-flex align-items-center justify-content-center min-vh-100">
    <div class="row w-100 justify-content-center">
      <div class="col-md-6 col-lg-4">
        <div class="login-container p-4">
          <h1 class="text-center mb-4">Inicio de Sesión</h1>
          <p v-if="errorMessage" class="text-danger text-center">{{ errorMessage }}</p>

          <form @submit.prevent="handleLogin">
            <div class="mb-3">
              <label for="username" class="form-label">Username</label>
              <input
                id="username"
                v-model="username"
                type="text"
                class="form-control"
                placeholder="Enter your username"
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

            <button type="submit" class="btn btn-primary w-100" :disabled="isLoggingIn">
              <span v-if="isLoggingIn" class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
              <span v-if="isLoggingIn"> Iniciando Sesión...</span>
              <span v-else>Iniciar Sesión</span>
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
const isLoggingIn = ref(false)

const router = useRouter()

interface LoginSuccessResponse {
  id: number;
  username: string;
  email: string;
  firstName: string;
  lastName: string;
  gender: string;
  image: string;
  accessToken: string;
  refreshToken: string;
}

interface LoginErrorResponse {
  message: string;
}

type LoginResponse = LoginSuccessResponse | LoginErrorResponse;

function isLoginSuccess(response: LoginResponse): response is LoginSuccessResponse {
  return (response as LoginSuccessResponse).accessToken !== undefined;
}

async function handleLogin() {
  errorMessage.value = ''
  isLoggingIn.value = true
  try {
    const response = await fetch('https://dummyjson.com/auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        username: username.value,
        password: password.value
      })
    })
    const data: LoginResponse = await response.json()

    if (!response.ok) {
      if ('message' in data) {
        errorMessage.value = data.message || 'Error en la autenticación'
      } else {
        errorMessage.value = 'Error en la autenticación'
      }
      return
    }

    if (isLoginSuccess(data)) {
      localStorage.setItem('authToken', data.accessToken)
      router.push('/dashboard')
    } else {
      errorMessage.value = 'Respuesta inesperada de la autenticación'
    }
  } catch (err) {
    console.error(err)
    errorMessage.value = 'Error de conexión o de servidor'
  } finally {
    isLoggingIn.value = false
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

