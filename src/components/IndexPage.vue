<template>
  <div id="app">

    <section class="profile" v-if="about">
      <div class="profile__avatar">
        <img :src="about.selfie" alt="Foto Perfil" />
      </div>

      <div class="profile__info">
        <div class="profile__info-header">
          <p class="profile__info-header--name">{{ about.name }} {{ about.surname }}</p>
          <p class="profile__info-header--position">{{ about.position }}</p>
        </div>
        <div class="profile__info-description">
          <p class="profile__info-description--text">{{ about.description }}</p>
        </div>
      </div>
    </section>

    <section class="badges">
      <img v-for="badge in badges" :key="badge.id" :src="badge.badge" alt="badge" width="100" height="100" />
    </section>

  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const about = ref(null)
const badges = ref([])

onMounted(async () => {
  console.log('Mounted Indexpage.vue') // Ver si monta 1 sola vez
  try {
    const resAbout = await axios.get('https://portfolio-blackeagle.fly.dev/api/about/')
    console.log('About API response:', resAbout.data)
    if (resAbout.data && resAbout.data.length > 0) {
      about.value = resAbout.data[0]
    }
  } catch (e) {
    console.error('Error loading about:', e)
  }

  try {
    const resBadges = await axios.get('https://portfolio-blackeagle.fly.dev/api/badge/')
    console.log('Badges API response:', resBadges.data)
    badges.value = resBadges.data
  } catch (e) {
    console.error('Error loading badges: ', e)
  }
})
</script>


<style scoped>
@import '@/assets/css/style.css';
</style>
