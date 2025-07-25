<template>
  <teleport to="body">
    <div v-if="visible" class="modal" id="educationModal" @click.self="closeModal">
      <div class="modal-content" id="modalContent">
        <span class="close" @click="closeModal">&times;</span>
        <img
          class="modal-image"
          :src="education.image || ''"
          :alt="education.title || 'Education Image'"
          @error="handleImageError"
          @load="handleImageLoad"
        >
        <h2 class="modal-title" id="modalTitle">
          {{ education.title || 'Sin título' }}
        </h2>
        <h3 class="modal-institute" id="modalInstitute">
          {{ (education.institute || education.institution || 'Sin institución') }} - {{ education.year || 'Sin año' }}
        </h3>
        <p v-if="education.desccription || education.description" class="modal-description">
          {{ education.desccription || education.description }}
        </p>
        <div v-else class="modal-description">
          <em>Sin descripción disponible</em>
        </div>
      </div>
    </div>
  </teleport>
</template>

<script setup>
import { onMounted, onUnmounted, watch, nextTick } from 'vue'

const props = defineProps({
  education: {
    type: Object,
    default: () => ({})
  },
  visible: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['close'])

const closeModal = () => {
  console.log('Closing modal')
  emit('close')
}

const handleImageError = (event) => {
  console.warn('Error loading image:', event.target.src)
  console.log('Education data:', props.education)
  // Mostrar placeholder si la imagen falla
  event.target.src = 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjAwIiBoZWlnaHQ9IjIwMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cmVjdCB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiBmaWxsPSIjZGRkIi8+PHRleHQgeD0iNTAlIiB5PSI1MCUiIGZvbnQtZmFtaWx5PSJBcmlhbCwgc2Fucy1zZXJpZiIgZm9udC1zaXplPSIxNCIgZmlsbD0iIzk5OSIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZHk9Ii4zZW0iPkltYWdlbiBubyBkaXNwb25pYmxlPC90ZXh0Pjwvc3ZnPg=='
}

const handleImageLoad = () => {
  console.log('Image loaded successfully:', props.education.image)
}

const handleKeyDown = (event) => {
  if (event.key === 'Escape' && props.visible) {
    closeModal()
  }
}

// Prevenir scroll del body cuando el modal está abierto
watch(() => props.visible, (newVal) => {
  if (newVal) {
    document.body.style.overflow = 'hidden'
    nextTick(() => {
      console.log('Modal opened, education data:', props.education)
    })
  } else {
    document.body.style.overflow = 'auto'
  }
})

onMounted(() => {
  document.addEventListener('keydown', handleKeyDown)
})

onUnmounted(() => {
  document.removeEventListener('keydown', handleKeyDown)
  // Restaurar scroll al desmontar el componente
  document.body.style.overflow = 'auto'
})
</script>

<style scoped>
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  width: 100vw;
  height: 100vh;
  transform: translate(-50%, -50%);
  background-color: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1004;
  overflow: auto;
  padding: 20px;
  box-sizing: border-box;
}

.close {
  position: absolute;
  top: 10px;
  right: 15px;
  color: #aaa;
  font-size: 28px;
  font-weight: bold;
  cursor: pointer;
  z-index: 1001;
  line-height: 1;
  background: rgba(255, 255, 255, 0.8);
  border-radius: 50%;
  width: 35px;
  height: 35px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  background: rgba(255, 255, 255, 1);
}

.modal-content {
  background-color: #fefefe;
  padding: 30px;
  border: none;
  border-radius: 12px;
  position: relative;
  animation: modalopen 0.4s ease-out;
  max-width: 600px;
  width: 100%;
  max-height: 90vh;
  overflow-y: auto;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
  margin: auto;
}

.modal-image {
  width: 100%;
  height: auto;
  max-height: 300px;
  object-fit: contain;
  border-radius: 8px;
  margin-bottom: 20px;
  display: block;
  background: #f5f5f5;
}

.modal-title {
  font-size: 1.8rem;
  font-weight: 600;
  margin: 0 0 1rem 0;
  color: #333;
  line-height: 1.3;
}

.modal-institute {
  font-size: 1.2rem;
  font-weight: 500;
  margin: 0 0 1.5rem 0;
  color: #666;
}

.modal-description {
  font-size: 1rem;
  line-height: 1.6;
  color: #555;
  margin: 0;
  text-align: left;
}

@keyframes modalopen {
  from {
    opacity: 0;
    transform: scale(0.8) translateY(-50px);
  }
  to {
    opacity: 1;
    transform: scale(1) translateY(0);
  }
}

@media (max-width: 768px) {
  .modal {
    padding: 10px;
  }

  .modal-content {
    padding: 20px;
    max-height: 95vh;
  }

  .close {
    font-size: 24px;
    width: 30px;
    height: 30px;
    top: 8px;
    right: 8px;
  }

  .modal-title {
    font-size: 1.4rem;
  }

  .modal-institute {
    font-size: 1rem;
  }
}
</style>
