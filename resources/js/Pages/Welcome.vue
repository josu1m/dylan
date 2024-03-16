<template>
  <div>
    <nav class="navbar-welcome">
      <ul class="navbar-links" :class="{ 'visible': isMenuVisible }">
        <li class="navbar-item">
          <Link class="navbar-link" href="#">Inicio</Link>
        </li>
        <li class="navbar-item">
          <Modo />
        </li>
        <li class="navbar-item">
          <Link class="navbar-link" href="#">Servicios</Link>
        </li>
      </ul>
      <button class="toggle-button" @click="toggleMenu" v-show="isSmallScreen">Toggle Navbar</button>
    </nav>
  </div>
</template>

<script setup>
import { Link } from "@inertiajs/vue3";
import Modo from "@/Components/Modo.vue";
import { ref, computed, onMounted, onUnmounted } from "vue";

const isMenuVisible = ref(false);

function toggleMenu() {
  isMenuVisible.value = !isMenuVisible.value; // Cambiar el estado del menú al hacer clic en el botón
}

const isSmallScreen = ref(window.innerWidth <= 600);

// Escuchar cambios en el tamaño de la ventana y actualizar la variable isSmallScreen
const updateWindowSize = () => {
  isSmallScreen.value = window.innerWidth <= 600;
};

onMounted(() => {
  window.addEventListener("resize", updateWindowSize);
});

onUnmounted(() => {
  window.removeEventListener("resize", updateWindowSize);
});
</script>

<style scoped>
.navbar-welcome {
  background-color: #ff0077;
  padding: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: relative;
}

.toggle-button {
  background-color: #ddd;
  color: #333;
  border: none;
  padding: 5px 10px;
  border-radius: 10px;
  cursor: pointer;
  display: none;
}

.navbar-links {
  list-style: none;
  padding: 0;
  margin: 0;
  background-color: #ff0077;
  display: flex;
  align-items: center;
  position: relative;
}

.navbar-item {
  margin-right: 10px;
}

.navbar-link {
  text-decoration: none;
  color: #fff;
  font-size: 16px;
  padding: 7px 10px;
  border-radius: 10px;
  background-color: #555;
}

.navbar-link:last-child {
  margin-right: 0;
}

.navbar-link:hover {
  background-color: #777;
}

@media screen and (max-width: 600px) {
  .navbar-links {
    flex-direction: column;
    width: 105px;
    position: absolute;
    top: calc(100% + 0px);
    left: 0;
    z-index: 1000;
    padding: 10px;
    height: 100vh;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
    align-items: normal;
    display: none;
  }

  .navbar-item {
    margin-right: 0;
    margin-bottom: 10px;
  }

  .toggle-button {
    margin-top: 1px;
    display: block;
  }

  .visible {
    display: flex;
  }
}

</style>
