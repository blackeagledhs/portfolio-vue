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

/* Estilos responsivos para dispositivos móviles */
@media (max-width: 768px) {
.titles {
font-size: 1.75rem;
margin-bottom: 1.5rem;
padding: 0 1rem;
}

.education {
grid-template-columns: 1fr;
gap: 1rem;
padding: 0.5rem;
}

main {
padding: 1.5rem 0.75rem;
min-height: calc(100vh - 100px);
}
}

@media (max-width: 480px) {
.titles {
font-size: 1.5rem;
margin-bottom: 1rem;
padding: 0 0.5rem;
}

.education {
gap: 0.75rem;
padding: 0.25rem;
}

main {
padding: 1rem 0.5rem;
min-height: calc(100vh - 80px);
}
}

/* Para pantallas muy pequeñas (menos de 360px) */
@media (max-width: 360px) {
.titles {
font-size: 1.25rem;
word-break: break-word;
}

.education {
grid-template-columns: 1fr;
padding: 0;
}

main {
padding: 0.75rem 0.25rem;
}
}

/* Ajustes para orientación horizontal en móviles */
@media (max-height: 500px) and (orientation: landscape) {
main {
padding: 1rem 0.5rem;
min-height: calc(100vh - 60px);
margin-top: 20px;
}

.titles {
font-size: 1.5rem;
margin-bottom: 1rem;
}

.education {
gap: 1rem;
}
}
</style>
