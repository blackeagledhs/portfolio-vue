<template>
  <div
    class="card"
    @click="handleClick"
    :data-title="education.title"
    :data-institute="education.institute || education.institution"
    :data-year="education.year"
    :data-image="education.image"
    :data-description="education.desccription || education.description"
  >
    <div class="card-header">
      <h2 class="card-title">{{ education.title }}</h2>
      <h3 class="card-institute">
        {{ education.institute || education.institution }} - {{ education.year }}
      </h3>
    </div>
    <div class="card-content">
      <p class="card-description">
        {{ education.desccription || education.description }}
      </p>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  education: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['show-modal'])

const handleClick = () => {
  console.log('Card clicked:', props.education)
  emit('show-modal', props.education)
}
</script>

<style scoped>
.card {
  background: #fff;
  border-radius: 12px;
  padding: 1.5rem;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  border: 1px solid #e0e0e0;
}

.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
  border-color: #5f5f5f;
}

.card-header {
  margin-bottom: 1rem;
}

.card-title {
  font-size: 1.25rem;
  font-weight: 600;
  margin: 0 0 0.5rem 0;
  color: var(--text, #333);
  line-height: 1.3;
}

.card-institute {
  font-size: 0.95rem;
  font-weight: 500;
  margin: 0;
  color: var(--text-secondary, #666);
  opacity: 0.8;
}

.card-content {
  margin-top: 1rem;
}

.card-description {
  font-size: 0.9rem;
  line-height: 1.5;
  color: var(--text, #555);
  margin: 0;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
