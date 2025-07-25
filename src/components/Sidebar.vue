<template>
    <div class="sidebar-container">
        <!-- Sidebar -->
        <aside class="sidebar" :class="{ active: isSidebarActive }">
            <div class="sidebar-column">
                <div class="sidebar-buttons">
                    <router-link to="/education" @click="closeSidebar">
                        <button>Educación</button>
                    </router-link>
                    <router-link to="/experience" @click="closeSidebar">
                        <button>Experiencia</button>
                    </router-link>
                    <router-link to="/projects" @click="closeSidebar">
                        <button>Proyectos</button>
                    </router-link>
                    <router-link to="/extras" @click="closeSidebar">
                        <button>Extras</button>
                    </router-link>
                    <router-link to="/contact" @click="closeSidebar">
                        <button>Contacto</button>
                    </router-link>
                    <button @click="downloadCV" class="download-btn">Descargar CV</button>
                </div>
            </div>
            <div class="sidebar-column">
                <div class="sidebar-social">
                    <a href="https://github.com/blackeagledhs" class="social-link" target="_blank"><i class="bi bi-github"></i></a>
                    <a href="mailto:mauro.vicens@gmail.com" class="social-link"><i class="bi bi-envelope"></i></a>
                    <a href="https://linkedin.com/in/mauro-andres-vicens" class="social-link" target="_blank"><i class="bi bi-linkedin"></i></a>
                    <a href="https://instagram.com/mauro.vicens" class="social-link" target="_blank"><i class="bi bi-instagram"></i></a>
                    <a href="https://t.me/Black_Eagle_DHS" class="social-link" target="_blank"><i class="bi bi-telegram"></i></a>
                    <a href="https://wa.me/5493471630170" class="social-link" target="_blank"><i class="bi bi-whatsapp"></i></a>
                </div>
            </div>
        </aside>

        <!-- Hamburger Menu -->
        <div class="hamburger-menu" @click="toggleSidebar" :class="{ active: isSidebarActive }">
            <div class="bar"></div>
            <div class="bar"></div>
            <div class="bar"></div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'

const isSidebarActive = ref(false)

function toggleSidebar() {
    isSidebarActive.value = !isSidebarActive.value
}

function closeSidebar() {
    isSidebarActive.value = false
}

function downloadCV() {
    const link = document.createElement('a')
    link.href = '/cv/Mauro_Vicens_CV.pdf'
    link.setAttribute('download', 'CV-Mauro-Andres-Vicens.pdf')
    link.setAttribute('target', '_blank')
    document.body.appendChild(link)
    link.click()
    document.body.removeChild(link)
}
</script>

<style scoped>
.sidebar-container {
    position: relative;
}

.sidebar {
    position: fixed;
    top: 0;
    right: 0;
    width: 210px;
    background-color: #333;
    color: white;
    padding: 20px;
    height: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    gap: 10px;
    z-index: 999;
    transition: transform 0.3s ease-in-out;
}

.sidebar-column {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.sidebar-buttons {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin: auto 0;
}

.sidebar-buttons router-link {
    text-decoration: none;
}

.sidebar-buttons button,
.download-btn {
    background-color: #444;
    color: white;
    border: 1px solid transparent;
    padding: 10px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s, transform 0.3s ease;
    margin: 20px 0;
    font-weight: bold;
    color: #ffffff;
    transform: skew(-20deg);
    border-image-source: linear-gradient(135deg, #FF4500, #FFA500);
    border-image-slice: 1;
    width: 100%;
}

.sidebar-buttons button:hover,
.download-btn:hover {
    background-color: #555;
    transform: skew(-20deg) scale(1.1);
    background: linear-gradient(135deg, #FF4500, #FFA500);
}

.sidebar-social {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin: auto 0;
}

.social-link {
    color: white;
    text-decoration: none;
    padding: 10px;
    background-color: #444;
    border: 1px solid transparent;
    border-radius: 5px;
    text-align: center;
    transition: background-color 0.3s;
}

.social-link i {
    font-size: 20px;
}

.social-link:hover {
    background-color: #555;
}

/* Hamburger Menu */
.hamburger-menu {
    display: none;
    position: fixed;
    top: 15px;
    right: 15px;
    z-index: 1000;
    cursor: pointer;
    width: 30px;
    height: 24px;
    flex-direction: column;
    justify-content: space-between;
}

.bar {
    width: 100%;
    height: 4px;
    background-color: #333;
    transition: transform 0.3s ease-in-out, background-color 0.3s ease-in-out;
    border-radius: 2px;
}

.hamburger-menu:hover .bar {
    background-color: #FF4500;
}

/* Animación del hamburger cuando está activo */
.hamburger-menu.active .bar:nth-child(1) {
    transform: rotate(45deg) translate(5px, 5px);
}

.hamburger-menu.active .bar:nth-child(2) {
    opacity: 0;
}

.hamburger-menu.active .bar:nth-child(3) {
    transform: rotate(-45deg) translate(7px, -6px);
}

/* Responsive Design */
@media screen and (max-width: 768px) {
    .sidebar {
        transform: translateX(100%);
        width: 100%;
        max-width: 300px;
    }

    .sidebar.active {
        transform: translateX(0);
    }

    .hamburger-menu {
        display: flex;
    }

    .sidebar-buttons {
        gap: 15px;
    }

    .sidebar-buttons button,
    .download-btn {
        margin: 10px 0;
        transform: skew(0deg);
        font-size: 16px;
    }

    .sidebar-buttons button:hover,
    .download-btn:hover {
        transform: skew(0deg) scale(1.05);
    }
}

@media screen and (max-width: 480px) {
    .sidebar {
        width: 100%;
        max-width: 100%;
    }

    .hamburger-menu {
        top: 10px;
        right: 10px;
    }
}
</style>
