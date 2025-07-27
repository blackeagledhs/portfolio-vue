<template>
  <div>

    <main class="responsive-container">
      <section class="extra-section">
        <div class="extra-card">
          <div
            v-for="extra in extras"
            :key="extra.id || extra.year + extra.activity"
            class="extra-item"
          >
            <span class="year">{{ extra.year }}</span> -
            <span class="activity">{{ extra.activity }}</span>
          </div>

          <div v-if="extras.length === 0 && !loadingExtras" class="no-data">
            No hay datos extras disponibles
          </div>

          <div v-if="loadingExtras" class="loading">
            Cargando extras...
          </div>
        </div>
      </section>

      <section class="other-section">
        <div
          v-for="other in others"
          :key="other.id || other.title"
          class="other-card"
        >
          <img
            :src="other.image"
            :alt="`Imagen de ${other.title}`"
            @error="handleImageError"
          >
          <h2>{{ other.title }}</h2>
          <p class="institute-year">{{ other.institute }} - {{ other.year }}</p>
        </div>

        <div v-if="others.length === 0 && !loadingOthers" class="no-data">
          No hay otros datos disponibles
        </div>

        <div v-if="loadingOthers" class="loading">
          Cargando otros datos...
        </div>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'


const extras = ref([])
const others = ref([])
const loadingExtras = ref(false)
const loadingOthers = ref(false)
const error = ref(null)


const fetchExtras = async () => {
  try {
    loadingExtras.value = true
    const response = await fetch('https://portfolio-blackeagle.fly.dev/api/extra/')

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }

    const data = await response.json()
    extras.value = data

  } catch (err) {
    console.error('Error fetching extras:', err)
    extras.value = []
    error.value = `Error cargando extras: ${err.message}`
  } finally {
    loadingExtras.value = false
  }
}


const fetchOthers = async () => {
  try {
    loadingOthers.value = true
    const response = await fetch('https://portfolio-blackeagle.fly.dev/api/other/')

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }

    const data = await response.json()
    others.value = data

  } catch (err) {
    console.error('Error fetching others:', err)
    others.value = []
    error.value = `Error cargando others: ${err.message}`
  } finally {
    loadingOthers.value = false
  }
}


const handleImageError = (event) => {
  console.error('Error loading image:', event.target.src)
  // Opcionalmente puedes asignar una imagen por defecto
  // event.target.src = '/path/to/default-image.jpg'
}


onMounted(async () => {
  await Promise.all([
    fetchExtras(),
    fetchOthers()
  ])
})
</script>

<style scoped>
body {
    font-family: Arial, sans-serif;
}

main {
    padding: 20px;
}


.extra-section {
    max-width: 1024px;
    display: flex;
    justify-content: center;
    margin: 0 auto 40px;
}

.extra-card {
    background-color: #f0f0f0;
    border-radius: 8px;
    padding: 20px;
    display: flex;
    flex-direction: column;
    gap: 10px;
    width: 100%;
}

.extra-item {
    background-color: #ccc;
    padding: 10px;
    border-radius: 8px;
    display: flex;
    justify-content: center;
}

.extra-item .year {
    font-weight: bold;
}

.extra-item .activity {
    text-align: left;
}


.other-section {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
    max-width: 1024px;
    margin: 0 auto;
}

.other-card {
    background-color: #f0f0f0;
    border-radius: 8px;
    padding: 20px;
    text-align: center;
}

.other-card img {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
    margin-bottom: 10px;
}

.other-card h2 {
    margin-top: 10px;
    margin-bottom: 10px;
}

.other-card .institute-year {
    font-style: italic;
    margin-top: 5px;
}


.loading {
    text-align: center;
    padding: 20px;
    color: #666;
    font-style: italic;
}

.no-data {
    text-align: center;
    padding: 20px;
    color: #999;
    font-style: italic;
}

/* Media query para pantallas pequeñas */
@media (max-width: 768px) {
    .extra-section {
        padding-top: 20px;
    }
    .extra-card {
        flex-direction: column;
    }

    .other-section {
        grid-template-columns: 1fr;
    }

    main {
        padding: 10px;
    }
}
</style>
