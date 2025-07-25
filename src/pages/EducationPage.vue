<template>
  <main>
    <h2 class="titles">Estudios y Cursos</h2>
    <section class="education">
      <EducationCard
        v-for="(edu, index) in educationList"
        :key="index"
        :education="edu"
        @show-modal="openModal"
      />
    </section>

    <EducationModal
      :education="selectedEducation"
      :visible="isModalVisible"
      @close="closeModal"
    />
  </main>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import EducationCard from '@/components/EducationCard.vue'
import EducationModal from '@/components/EducationModal.vue'
import axios from 'axios'

const educationList = ref([])
const selectedEducation = ref(null)
const isModalVisible = ref(false)

const openModal = (education) => {
  console.log('Opening modal with:', education)
  selectedEducation.value = education
  isModalVisible.value = true
}

const closeModal = () => {
  isModalVisible.value = false
  selectedEducation.value = null
}

onMounted(async () => {
  try {
    const res = await axios.get('https://portfolio-blackeagle.fly.dev/api/education/')
    console.log('API Response:', res.data)

    // Manejar diferentes estructuras de respuesta
    if (Array.isArray(res.data)) {
      educationList.value = res.data
    } else if (res.data.education && Array.isArray(res.data.education)) {
      educationList.value = res.data.education
    } else if (res.data.length && res.data[0].education) {
      educationList.value = res.data[0].education
    } else {
      console.warn('Unexpected API response structure:', res.data)
      educationList.value = []
    }

    console.log('Education list:', educationList.value)
  } catch (err) {
    console.error('Error fetching education data:', err)
    educationList.value = []
  }
})
</script>

<style scoped>
.titles {
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
  color: var(--text, #333);
}

.education {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.5rem;
  padding: 1rem;
  max-width: 1200px;
  margin: 0 auto;
}

main {
  padding: 2rem 1rem;
  min-height: calc(100vh - 120px); 
  max-width: 1200px;
  margin: 0 auto;
  box-sizing: border-box;
  position: relative;
}
</style>
