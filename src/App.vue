<script setup>
import Topbar from './components/Topbar.vue';
import Sidebar from './components/Sidebar.vue';
import Footer from './components/Footer.vue';
import { useRoute } from 'vue-router'
import { computed } from 'vue'

const route = useRoute()

// Determina si estamos en la página principal
const isHomePage = computed(() => route.path === '/')
</script>

<template>
  <div class="app-container">
    <!-- Topbar visible en todas las páginas excepto home -->
    <Topbar v-if="!isHomePage" class="topbar" />

    <!-- Sidebar solo visible en home -->
    <Sidebar v-if="isHomePage" />

    <div class="content-wrap" :class="{ 'with-topbar': !isHomePage }">
      <router-view />
    </div>

    <Footer class="footer" />
  </div>
</template>


<style scoped>
@import '@/assets/css/style.css';

/* Estructura principal */
.app-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  position: relative;
}


/* Ajuste de contenido cuando hay topbar */
.content-wrap.with-topbar {
  padding-top: 70px; 
}

/* Contenedor de contenido principal */
.content-wrap {
  flex: 1;
  padding-bottom: 80px; /* Ajusta según altura de tu footer */
}

/* Footer */
.footer {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  z-index: 100;
}

@media screen and (max-width: 768px) {
  .content-wrap {
    padding-bottom: 20px;
  }
  .content-wrap.with-topbar {
    padding-top: 50px;
  }
  .footer {
    position: relative;
  }
}
</style>
