<template>
  <div>
    <!-- Aquí incluirías tu componente topbar si lo tienes -->
    <!-- <Topbar /> -->

    <main>
      <section class="experience-section">
        <div
          v-for="experience in experiences"
          :key="experience.id || experience.position + experience.company"
          class="experience-card"
        >
          <div class="experience-header">
            <h2 class="position">{{ experience.position }}</h2>
            <h3 class="company">{{ experience.company }}</h3>
          </div>
          <div class="experience-date">
            {{ experience.month_of_income }}/{{ experience.year_of_income }} -
            {{ experience.month_of_egress }}/{{ experience.year_of_egress }}
          </div>
          <p class="description">{{ experience.description }}</p>
        </div>
      </section>
    </main>

    <!-- Aquí incluirías tu componente footer si lo tienes -->
    <!-- <Footer /> -->
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

// Estado reactivo para las experiencias
const experiences = ref([])
const loading = ref(true)
const error = ref(null)

// Función para obtener las experiencias desde la API usando Axios
const fetchExperiences = async () => {
  try {
    loading.value = true
    error.value = null

    const response = await axios.get('https://portfolio-blackeagle.fly.dev/api/experience/')
    experiences.value = response.data
  } catch (err) {
    error.value = err.response?.data?.message || err.message || 'Error al cargar experiencias'
    console.error('Error fetching experience:', err)
    experiences.value = [] // Array vacío en caso de error
  } finally {
    loading.value = false
  }
}

// Ejecutar al montar el componente
onMounted(() => {
  fetchExperiences()
})

// Opcional: función para recargar datos
const reloadExperiences = () => {
  fetchExperiences()
}

// Exponer funciones si necesitas usarlas desde el componente padre
defineExpose({
  reloadExperiences
})
</script>

<style scoped>
/* Estilos exactos del CSS original mantenidos */
body {
    font-family: Arial, sans-serif;
}

.experience-section {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    max-width: 1200px; /* Controla el ancho máximo para 3 columnas */
    margin: 0 auto;
    gap: 16px;
    padding: 16px;
}

/* Media query para asegurar máximo 3 columnas */
@media (min-width: 1200px) {
    .experience-section {
        grid-template-columns: repeat(3, 1fr);
    }
}

.experience-card {
    background-color: #f9f9f9;
    border: 1px solid #ddd;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    padding: 0;  /* Remove padding from card itself to control it within elements */
    width: 100%;
    min-width: 300px; /* Ancho mínimo para mantener legibilidad */
}

.experience-header {
    background-color: #666666;  /* Dark grey */
    color: #ffffff;  /* White text */
    padding: 16px;
    border-top-left-radius: 8px;
    border-top-right-radius: 8px;
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    min-height: 80px; /* Altura mínima uniforme */
}

.experience-header h2,
.experience-header h3 {
    margin: 0;
    line-height: 1.3;
    word-wrap: break-word;
    overflow-wrap: break-word;
}

.experience-header h2 {
    font-size: 1.2em;
    font-weight: bold;
    flex: 1;
    margin-right: 8px;
}

.experience-header h3 {
    font-size: 1em;
    font-weight: normal;
    opacity: 0.9;
    text-align: right;
    flex-shrink: 0;
    max-width: 40%;
}

.experience-date {
    font-size: 0.9em;
    color: #555;
    margin-top: 8px;
    margin-bottom: 8px;
    padding: 0 16px;  /* Ensure alignment with card padding */
}

.description {
    font-size: 1em;
    color: #333;
    padding: 0 16px 16px;  /* Ensure alignment with card padding */
}

/* Estilos responsivos */
@media (max-width: 768px) {
    .experience-section {
        grid-template-columns: 1fr; /* Una sola columna en móviles */
        max-width: 100%;
        padding-top: 20px;
    }

    .experience-card {
        min-width: auto;
    }

    .experience-header {
        flex-direction: column;
        align-items: flex-start;
    }

    .experience-header h2,
    .experience-header h3 {
        margin-bottom: 8px;
    }
}

@media (min-width: 769px) and (max-width: 1199px) {
    .experience-section {
        grid-template-columns: repeat(2, 1fr); /* Dos columnas en tablets */
    }
}

/* Estilos adicionales para estados de carga y error (opcional) */
.loading {
    text-align: center;
    padding: 2rem;
    color: #666;
}

.error {
    text-align: center;
    padding: 2rem;
    color: #d32f2f;
    background-color: #ffebee;
    border-radius: 4px;
    margin: 1rem;
}
</style>
