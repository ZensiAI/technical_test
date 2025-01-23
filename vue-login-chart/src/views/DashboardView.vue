<template>
    <div class="dashboard-container">
      <h2>Dashboard - Gráfico del Dólar Anual</h2>
  
      <button @click="logout" class="btn btn-secondary mb-3">
        Cerrar Sesión
      </button>
  
      <p v-if="loading">Cargando datos...</p>
      <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
  
      <apexchart
        v-if="!loading && !errorMessage && chartSeries[0].data.length"
        type="line"
        height="350"
        :options="chartOptions"
        :series="chartSeries"
      />
    </div>
  </template>
  
  <script setup lang="ts">
  import { onMounted, ref } from 'vue'
  import { useRouter } from 'vue-router'
  
  const loading = ref(false)
  const errorMessage = ref('')
  
  const chartOptions = ref({
    chart: {
      id: 'dolar-chart'
    },
    xaxis: {
      categories: [] as string[]
    },
    yaxis: {
      title: {
        text: 'Valor Dólar'
      }
    },
    title: {
      text: 'Dólar Histórico (Anual)',
      align: 'center'
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

      const dates = responseJson.data.map((item: any) => item.fecha)
      const values = responseJson.data.map((item: any) => parseFloat(item.valor))
  
      console.log('Fechas:', dates)
      console.log('Valores numéricos:', values)
      
      chartOptions.value.xaxis.categories = dates
      chartSeries.value[0].data = values
    } catch (err: any) {
      console.error(err)
      errorMessage.value = err.message || 'Error al cargar datos'
    } finally {
      loading.value = false
    }
  }
  </script>
  
  <style scoped>
  .dashboard-container {
    margin: 1rem auto;
    max-width: 600px;
    text-align: center;
  }
  
  .error {
    color: red;
  }
  </style>







  