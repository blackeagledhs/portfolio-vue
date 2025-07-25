<template>
  <main>
    <section class="projects-section">
      <!-- Slider para desktop -->
      <div class="slider-container">
        <div class="slider" ref="slider">
          <div
            v-for="project in projects"
            :key="project.id"
            class="slide"
          >
            <div class="slide-content">
              <div class="image-container">
                <img :src="project.image" :alt="project.project_name">
                <div class="image-overlay"></div>
              </div>
              <div class="text-content">
                <h2>{{ project.project_name }}</h2>
                <p>{{ project.description }}</p>
                <a
                  :href="project.project_url"
                  class="project-btn"
                  target="_blank"
                >
                  <span>Ver Proyecto</span>
                  <i class="arrow">→</i>
                </a>
              </div>
            </div>
          </div>
        </div>

        <!-- Indicadores de slide -->
        <div class="slide-indicators">
          <button
            v-for="(project, index) in projects"
            :key="`indicator-${project.id}`"
            :class="['indicator', { active: index === currentSlideIndex }]"
            @click="goToSlide(index)"
          ></button>
        </div>

        <!-- Controles de navegación -->
        <button class="nav-btn prev" @click="prevSlide">‹</button>
        <button class="nav-btn next" @click="nextSlide">›</button>
      </div>

      <!-- Cards para móvil -->
      <div class="project_cards">
        <div
          v-for="project in projects"
          :key="`card-${project.id}`"
          class="project_card"
        >
          <img :src="project.image" :alt="project.project_name">
          <div class="text-content">
            <h2>{{ project.project_name }}</h2>
            <p>{{ project.description }}</p>
            <a
              :href="project.project_url"
              class="project-btn"
              target="_blank"
            >
              Ver Proyecto
            </a>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// Datos reactivos
const projects = ref([])
const slider = ref(null)
const currentSlideIndex = ref(0)
let slideInterval = null

// Función para obtener proyectos desde la API
const fetchProjects = async () => {
  try {
    const response = await fetch('https://portfolio-blackeagle.fly.dev/api/projects/')
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }
    const data = await response.json()
    projects.value = data
  } catch (error) {
    console.error('Error fetching projects:', error)
    projects.value = []
  }
}

// Función para mostrar el siguiente slide
const showNextSlide = () => {
  if (!slider.value || projects.value.length === 0) return

  const slides = slider.value.querySelectorAll('.slide')
  if (slides.length === 0) return

  currentSlideIndex.value = (currentSlideIndex.value + 1) % slides.length
  const slideWidth = slides[0].clientWidth
  const offset = currentSlideIndex.value * slideWidth
  slider.value.style.transform = `translateX(-${offset}px)`
}

// Función para ir a un slide específico
const goToSlide = (index) => {
  if (!slider.value || projects.value.length === 0) return

  const slides = slider.value.querySelectorAll('.slide')
  if (slides.length === 0) return

  currentSlideIndex.value = index
  const slideWidth = slides[0].clientWidth
  const offset = currentSlideIndex.value * slideWidth
  slider.value.style.transform = `translateX(-${offset}px)`

  // Reiniciar el auto-slider
  stopSlider()
  startSlider()
}

// Función para ir al slide anterior
const prevSlide = () => {
  if (!slider.value || projects.value.length === 0) return

  const slides = slider.value.querySelectorAll('.slide')
  if (slides.length === 0) return

  currentSlideIndex.value = currentSlideIndex.value === 0
    ? slides.length - 1
    : currentSlideIndex.value - 1

  const slideWidth = slides[0].clientWidth
  const offset = currentSlideIndex.value * slideWidth
  slider.value.style.transform = `translateX(-${offset}px)`

  // Reiniciar el auto-slider
  stopSlider()
  startSlider()
}

// Función para ir al slide siguiente
const nextSlide = () => {
  showNextSlide()
}

// Función para ajustar la posición del slider al redimensionar
const handleResize = () => {
  if (!slider.value || projects.value.length === 0) return

  const slides = slider.value.querySelectorAll('.slide')
  if (slides.length === 0) return

  const slideWidth = slides[0].clientWidth
  const offset = currentSlideIndex.value * slideWidth
  slider.value.style.transform = `translateX(-${offset}px)`
}

// Inicializar el slider automático
const startSlider = () => {
  slideInterval = setInterval(showNextSlide, 3000)
}

// Detener el slider automático
const stopSlider = () => {
  if (slideInterval) {
    clearInterval(slideInterval)
    slideInterval = null
  }
}

// Hooks del ciclo de vida
onMounted(async () => {
  await fetchProjects()

  // Iniciar el slider después de que los proyectos se hayan cargado
  if (projects.value.length > 0) {
    // Pequeño delay para asegurar que el DOM esté completamente renderizado
    setTimeout(() => {
      startSlider()
    }, 100)
  }

  // Agregar listener para resize
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  stopSlider()
  window.removeEventListener('resize', handleResize)
})
</script>

<style scoped>
body {
    font-family: Arial, sans-serif;
}

.projects-section {
    width: 100%;
    overflow: hidden;
    position: relative;
    background: linear-gradient(135deg, #f3e5db44, #e8d5ca44);
    padding: 40px 20px;
}

.slider-container {
    position: relative;
    width: 100%;
    overflow: hidden;
    border-radius: 20px;
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
}

.slider {
    display: flex;
    transition: transform 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.slide {
    min-width: 50%;
    box-sizing: border-box;
    padding: 20px;
    position: relative;
    flex: 0 0 50%;
}

.slide-content {
    padding: 24px;
    border-radius: 16px;
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    position: relative;
    transition: transform 0.3s ease;
    height: 100%;
}

.slide-content:hover {
    transform: translateY(-5px);
}

.image-container {
    position: relative;
    width: 100%;
    overflow: hidden;
    border-radius: 12px;
    margin-bottom: 20px;
}

.slide img {
    max-width: 100%;
    height: 300px;
    border-radius: 12px;
    object-fit: cover;
    transition: transform 0.5s ease;
}

.slide-content:hover .slide img {
    transform: scale(1.05);
}

.image-overlay {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(45deg, rgba(51, 51, 51, 0.2), rgba(119, 119, 119, 0.1));
    opacity: 0;
    transition: opacity 0.3s ease;
    border-radius: 12px;
}

.slide-content:hover .image-overlay {
    opacity: 1;
}

.text-content {
    background: linear-gradient(145deg, #2a2a2a, #404040);
    color: #fff;
    padding: 24px;
    margin-top: 0;
    border-radius: 12px;
    text-align: center;
    width: 100%;
    position: relative;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    min-height: 180px;
    flex: 1;
}

.text-content::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 2px;
    background: linear-gradient(90deg, transparent, #777, transparent);
}

.text-content h2 {
    font-size: 1.6em;
    margin: 0 0 12px 0;
    background: linear-gradient(135deg, #fff, #ccc);
    background-clip: text;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    font-weight: 700;
    flex-shrink: 0;
}

.text-content p {
    font-size: 1em;
    margin: 0 0 24px 0;
    color: #ccc;
    line-height: 1.5;
    display: flex;
    align-items: center;
    justify-content: center;
    flex: 1;
    text-align: center;
}

.text-content .project-btn {
    margin-top: auto;
    flex-shrink: 0;
}

.project-btn {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 12px 24px;
    background: linear-gradient(135deg, #555, #777);
    color: #fff;
    text-decoration: none;
    border-radius: 25px;
    font-weight: 600;
    transition: all 0.3s ease;
    position: relative;
    overflow: hidden;
    align-self: center;
    width: fit-content;
}

.project-btn::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
    transition: left 0.5s ease;
}

.project-btn:hover::before {
    left: 100%;
}

.project-btn:hover {
    background: linear-gradient(135deg, #666, #888);
    transform: translateY(-2px);
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
}

.project-btn .arrow {
    transition: transform 0.3s ease;
}

.project-btn:hover .arrow {
    transform: translateX(4px);
}

/* Indicadores modernos */
.slide-indicators {
    display: flex;
    justify-content: center;
    gap: 12px;
    padding: 20px 0;
    position: absolute;
    bottom: -60px;
    left: 50%;
    transform: translateX(-50%);
}

.indicator {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    border: 2px solid #999;
    background: transparent;
    cursor: pointer;
    transition: all 0.3s ease;
    position: relative;
}

.indicator.active {
    background: #777;
    transform: scale(1.2);
    box-shadow: 0 0 20px rgba(119, 119, 119, 0.5);
}

.indicator:hover {
    background: #666;
    transform: scale(1.1);
}

/* Botones de navegación modernos */
.nav-btn {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    width: 50px;
    height: 50px;
    border: none;
    border-radius: 50%;
    background: linear-gradient(135deg, rgba(85, 85, 85, 0.9), rgba(119, 119, 119, 0.9));
    color: white;
    font-size: 24px;
    cursor: pointer;
    transition: all 0.3s ease;
    display: flex;
    align-items: center;
    justify-content: center;
    backdrop-filter: blur(10px);
    z-index: 10;
}

.nav-btn:hover {
    background: linear-gradient(135deg, rgba(102, 102, 102, 0.95), rgba(136, 136, 136, 0.95));
    transform: translateY(-50%) scale(1.1);
    box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
}

.nav-btn.prev {
    left: 20px;
}

.nav-btn.next {
    right: 20px;
}

.project_cards {
    display: none;
    flex-wrap: wrap;
    gap: 20px;
}

.project_card {
    min-width: 50%;
    box-sizing: border-box;
    padding: 16px;
    position: relative;
    flex: 0 0 50%;
    border: 1px solid #ccc;
}

.project_card-content {
    background-color: #999999;
    padding: 16px;
    border-radius: 8px;
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
}

.project_card img {
    max-width: 100%;
    height: 300px;
    border-radius: 8px;
    object-fit: cover;
}

@media (max-width: 768px) {
    .projects-section {
        padding: 20px 10px;
    }

    .slider-container {
        border-radius: 16px;
        margin-bottom: 60px;
    }

    .slide {
        flex-basis: 100%;
        flex: 0 0 100%;
        padding: 12px;
    }

    .slide-content {
        padding: 16px;
    }

    .slide img {
        height: 200px;
    }

    .text-content {
        padding: 16px;
        margin-top: 0;
        min-height: 160px;
    }

    .text-content h2 {
        font-size: 1.3em;
    }

    .text-content p {
        font-size: 0.9em;
        margin-bottom: 20px;
    }

    .project-btn {
        padding: 10px 20px;
        font-size: 0.9em;
    }

    .slider-container {
        display: none;
    }

    .project_cards {
        display: flex;
        padding: 0 10px;
    }

    .project_card {
        flex-basis: 100%;
        flex: 0 0 100%;
        padding: 12px;
        margin: 0 auto;
        max-width: 95%;
        border-radius: 16px;
        border: none;
        overflow: hidden;
        transition: transform 0.3s ease;
    }

    .project_card:hover {
        transform: translateY(-3px);
    }

    .project_card img {
        display: block;
        margin: 0 auto;
        height: 200px;
        border-radius: 12px;
        transition: transform 0.3s ease;
    }

    .project_card:hover img {
        transform: scale(1.02);
    }

    .nav-btn {
        display: none;
    }

    .slide-indicators {
        display: none;
    }
}

@media (min-width: 769px) {
    .project_cards {
        display: none;
    }

    .slider-container {
        display: block;
        margin-bottom: 80px;
    }

    .slider {
        display: flex;
    }

    .slide {
        flex-basis: 50%;
        flex: 0 0 50%;
    }

    .nav-btn {
        display: flex;
    }

    .slide-indicators {
        display: flex;
    }
}
</style>
