<template>
  <header class="topbar">
    <div class="topbar-container">
      <nav>
        <ul :class="{ active: isMenuOpen }">
          <li v-for="(item, index) in navItems" :key="index">
            <router-link
              v-if="!item.download"
              :to="item.path"
              @click="closeMenu"
            >
              <span class="link-content">{{ item.text }}</span>
            </router-link>
            <a
              v-else
              :href="item.path"
              @click="closeMenu"
              download
            >
              <span class="link-content">{{ item.text }}</span>
            </a>
          </li>
        </ul>
        <div class="hamburger" @click="toggleMenu" :class="{ active: isMenuOpen }">
          <div></div>
          <div></div>
          <div></div>
        </div>
      </nav>
    </div>
  </header>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const baseUrl = import.meta.env.BASE_URL;
const isMenuOpen = ref(false);

// Configuración completa de navegación
const navItems = [
  { path: '/', text: 'Inicio' },
  { path: '/education', text: 'Educación' },
  { path: '/experience', text: 'Experiencia' },
  { path: '/projects', text: 'Proyectos' },
  { path: '/extras', text: 'Extras' },
  { path: '/contact', text: 'Contacto' },
  {
    path: `${baseUrl}cv/CV-Mauro-A-Vicens.pdf`,
    text: 'Descargar CV',
    download: true
  }
];

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
};

const closeMenu = () => {
  isMenuOpen.value = false;
};

// Función para aplicar el padding-top al body
const updateBodyPadding = () => {
  const topbar = document.querySelector('.topbar');
  if (topbar) {
    const topbarHeight = topbar.offsetHeight;
    document.body.style.paddingTop = `${topbarHeight}px`;
  }
};

onMounted(() => {
  // Aplicar padding inicial
  updateBodyPadding();

  // Actualizar padding cuando cambie el tamaño de ventana
  window.addEventListener('resize', updateBodyPadding);
});

onUnmounted(() => {
  // Limpiar el event listener
  window.removeEventListener('resize', updateBodyPadding);

  // Opcional: remover el padding del body
  document.body.style.paddingTop = '';
});
</script>

<style scoped>
/* Estilos base del topbar - 100% ancho y fijo */
.topbar {
  width: 100%;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1000;
  background: linear-gradient(87deg, #333, #414040, #ff6600);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
  padding: 15px 0;
  /* Altura mínima para cálculos consistentes */
  min-height: 70px;
}

.topbar-container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding-right: 15px;
}

.topbar nav {
  width: 100%;
  position: relative;
}

/* Estilos para la lista de navegación */
.topbar nav ul {
  list-style-type: none;
  display: flex;
  justify-content: center;
  margin: 0;
  padding: 0;
  width: 100%;
}

.topbar nav ul li {
  margin-right: 10px;
  position: relative;
}

.topbar nav ul li:last-child {
  margin-right: 15px;
}

/* Estilos para los links */
.topbar nav ul li a {
  color: #ffffff;
  font-weight: 700;
  text-decoration: none;
  padding: 10px 15px;
  display: block;
  position: relative;
  overflow: hidden;
  border: 1px solid #333;
  transform: skew(-20deg);
  transition: all 0.3s ease;
}

.link-content {
  display: inline-block;
  transform: skew(20deg);
}

.topbar nav ul li a::before {
  content: '';
  position: absolute;
  top: 50%;
  left: -10px;
  width: calc(100% + 20px);
  height: 100%;
  background: transparent;
  transform: skew(-20deg) translateY(-50%);
  z-index: -1;
  transition: transform 0.3s, background 0.3s;
}

.topbar nav ul li a:hover::before {
  background: #ff6600;
  transform: skew(-20deg) translateY(-50%) scale(1.1);
}

/* Estilo especial para el botón de descarga */
.topbar nav ul li a[download] {
  background-color: rgba(255, 102, 0, 0.2);
  border-color: #ff6600;
}

.topbar nav ul li a[download]:hover::before {
  background: #ff6600;
}

/* Estilos para móvil */
@media (max-width: 768px) {
  .topbar-container {
    padding-right: 20px;
  }

  .topbar {
    min-height: 50px;
    padding: 10px 0;
  }

  .topbar nav ul {
    flex-direction: column;
    align-items: flex-start;
    padding: 80px 0 20px; /* Más padding superior para evitar solapamiento */
    display: none;
    position: absolute;
    top: 0;
    left: 0;
    background: linear-gradient(87deg, #333, #414040, #ff6600);
    width: 100%;
    /* Altura mínima para el menú desplegable */
    min-height: 100vh;
  }

  .topbar nav ul.active {
    display: flex;
  }

  .topbar nav ul li {
    margin-right: 0;
    margin-bottom: 10px;
    width: 100%;
  }

  .topbar nav ul li:last-child {
    margin-right: 0;
  }

  .topbar nav ul li a {
    font-size: 16px;
    padding: 15px 20px; /* Más padding para mejor usabilidad táctil */
    border: 1px solid #ff6600;
    margin-left: 8px;
    transform: skew(0);
    width: calc(100% - 40px);
  }

  .link-content {
    transform: skew(0);
  }

  .topbar nav ul li a::before {
    transform: skew(0) translateY(-50%);
  }

  .topbar nav ul li a:hover::before {
    transform: skew(0) translateY(-50%) scale(1.1);
  }

  .hamburger {
    display: block;
    cursor: pointer;
    width: 30px;
    height: 30px;
    position: absolute;
    top: 20px; /* Centrado mejor en móvil */
    right: 20px;
    z-index: 1001;
  }

  .hamburger div {
    width: 100%;
    height: 4px;
    background-color: white;
    margin: 5px 0;
    transition: all 0.3s;
  }

  .hamburger.active div:nth-child(1) {
    transform: rotate(45deg) translate(5px, 5px);
  }

  .hamburger.active div:nth-child(2) {
    opacity: 0;
  }

  .hamburger.active div:nth-child(3) {
    transform: rotate(-45deg) translate(5px, -5px);
  }
}

/* Hamburger oculto en desktop */
@media (min-width: 769px) {
  .hamburger {
    display: none;
  }
}
</style>
