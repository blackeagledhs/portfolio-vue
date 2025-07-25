<template>
  <footer class="footer">
    <p>&copy; 2025 Mauro Vicens</p>
  </footer>
</template>

<script setup>
// Lógica si es necesaria
</script>

<style scoped>
.footer {
  width: 100%;
  background-color: #333;
  color: white;
  text-align: center;
  padding: 20px 0; /* ✅ Mide esta altura exacta (ej: 20px padding + línea de texto = ~60px total) */
  position: fixed;
  bottom: 0;
  left: 0;
  z-index: 1000;
  height: 60px; /* ✅ Altura EXPLÍCITA (ajústala a tu caso real) */
}

@media screen and (max-width: 768px) {
  .footer {
    position: relative;
    height: auto; /* ✅ Altura flexible en móvil */
    margin-top: 40px; /* ✅ Espacio extra antes del footer */
  }
  main {
    padding-bottom: 20px; /* ✅ Reduce espacio en móvil */
  }
}
</style>
