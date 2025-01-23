<template>
  <div class="dashboard-container position-relative p-3">
    <h2 class="mt-4 mb-5">Dashboard - Gráfico del Dólar Anual</h2>

    <button
      @click="logout"
      class="btn btn-secondary position-absolute top-0 end-0 m-3"
    >
      Cerrar Sesión
    </button>

    <p v-if="loading">Cargando datos...</p>
    <p v-if="errorMessage" class="error">{{ errorMessage }}</p>

    <apexchart
      v-if="!loading && !errorMessage && chartSeries[0].data.length"
      type="line"
      :width="chartWidth"
      :height="chartHeight"
      :options="chartOptions"
      :series="chartSeries"
      class="mb-5" 
    />
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import { useRouter } from 'vue-router'

interface DolarData {
  date: string;
  value: string; 
}

const loading = ref(false)
const errorMessage = ref('')

const chartWidth = ref('100%')
const chartHeight = ref('500px') 

const chartOptions = ref({
  chart: {
    id: 'dolar-chart'
  },
  title: {
    text: 'Dólar Histórico (Anual)',
    align: 'center',
    offsetY: 20
  },
  xaxis: {
    categories: [] as string[]
  },
  yaxis: {
    title: {
      text: 'Valor Dólar'
    }
  }
})

const chartSeries = ref([
  {
    name: 'Dólar',
    data: [] as number[]
  }
])

const router = useRouter()

onMounted(() => {
  checkAuth()
  fetchDolarData()
})

function checkAuth() {
  const token = localStorage.getItem('authToken')
  if (!token) {
    router.push('/')
  }
}

function logout() {
  localStorage.removeItem('authToken')
  router.push('/')
}

async function fetchDolarData() {
  loading.value = true
  errorMessage.value = ''

  try {
    const year = 2023
    const response = await fetch(`https://api.boostr.cl/economy/indicator/dolar/${year}.json`)

    if (!response.ok) {
      throw new Error(`Fallo al obtener datos del dólar (status: ${response.status})`)
    }

    const responseJson = await response.json()
    console.log('API Response:', responseJson)

    if (responseJson.status !== 'success') {
      throw new Error('La respuesta no indica "success"')
    }
    if (!Array.isArray(responseJson.data)) {
      throw new Error('La propiedad "data" no es un array')
    }

    const data: DolarData[] = responseJson.data;
    const dates = data.map((item) => item.date)
    const values = data.map((item) => parseFloat(item.value))

    chartOptions.value.xaxis.categories = dates
    chartSeries.value[0].data = values
  } catch (err: unknown) {
    if (err instanceof Error) {
      errorMessage.value = err.message || 'Error al cargar datos';
    } else {
      errorMessage.value = 'Error desconocido';
    }
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
.dashboard-container {
  max-width: 900px; 
  margin: 0 auto;
  text-align: center;
}

.error {
  color: red;
}
</style>







  