<template>
  <div>
    <div class="custom-dropdown">
      <button class="custom-dropbtn" @click="toggleDropdown">
        <i v-if="selectedTheme === 'system'" class="fas fa-desktop"></i>
        <i v-else-if="selectedTheme === 'light'" class="fas fa-sun"></i>
        <i v-else class="fas fa-moon"></i>
      </button>

      <div class="custom-dropdown-content" v-show="isDropdownOpen">
        <span @click="handleChangeThemeAndClose('system')"
          ><i class="fas fa-desktop"></i> Tema del sistema</span
        >
        <span @click="handleChangeThemeAndClose('light')"
          ><i class="fas fa-sun"></i> Tema claro</span
        >
        <span @click="handleChangeThemeAndClose('dark')"
          ><i class="fas fa-moon"></i> Tema oscuro</span
        >
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const isSystemThemeSelected = ref(false);
const isLightTheme = ref(false);
const isLightThemeActive = ref(true);
const selectedTheme = ref("system");
const isDropdownOpen = ref(false);

const detectTheme = () => {
  const systemTheme = window.matchMedia("(prefers-color-scheme: dark)").matches;
  isLightTheme.value = !systemTheme;
  if (isSystemThemeSelected.value) {
    applySystemTheme(systemTheme);
  }
};

const setLightTheme = () => {
  isSystemThemeSelected.value = false;
  isLightTheme.value = true;
  isLightThemeActive.value = true;
  localStorage.setItem("theme", "light");
  localStorage.setItem("themeSelection", "manual");
  document.body.classList.remove("tema-oscuro");
  document.body.classList.add("tema-claro");
};

const setDarkTheme = () => {
  isSystemThemeSelected.value = false;
  isLightTheme.value = false;
  isLightThemeActive.value = false;
  localStorage.setItem("theme", "dark");
  localStorage.setItem("themeSelection", "manual");
  document.body.classList.remove("tema-claro");
  document.body.classList.add("tema-oscuro");
};

const toggleDropdown = () => {
  isDropdownOpen.value = !isDropdownOpen.value;
};

const closeDropdown = () => {
  isDropdownOpen.value = false;
};

const toggleSystemThemeSelection = () => {
  isSystemThemeSelected.value = !isSystemThemeSelected.value;
  if (isSystemThemeSelected.value) {
    detectTheme();
    localStorage.setItem("themeSelection", "system");
  }
};

const applySystemTheme = (isDark) => {
  isLightTheme.value = !isDark;
  document.body.classList.toggle("tema-oscuro", isDark);
  document.body.classList.toggle("tema-claro", !isDark);
};

const handleChangeTheme = (theme) => {
  selectedTheme.value = theme;
  if (theme === "system") {
    toggleSystemThemeSelection();
  } else if (theme === "light") {
    setLightTheme();
  } else if (theme === "dark") {
    setDarkTheme();
  }
  localStorage.setItem("selectedTheme", theme);
};

const handleChangeThemeAndClose = (theme) => {
  handleChangeTheme(theme);
  closeDropdown();
};
onMounted(() => {
  const themeSelection = localStorage.getItem("themeSelection");
  if (themeSelection === null) {
    isSystemThemeSelected.value = true;
    detectTheme();
    localStorage.setItem("themeSelection", "system");
  } else if (themeSelection === "system") {
    isSystemThemeSelected.value = true;
    detectTheme();
  } else {
    const storedTheme = localStorage.getItem("theme");
    if (storedTheme === "light") {
      setLightTheme();
    } else if (storedTheme === "dark") {
      setDarkTheme();
    }
  }

  const storedSelectedTheme = localStorage.getItem("selectedTheme");
  if (storedSelectedTheme) {
    selectedTheme.value = storedSelectedTheme;
  }

  const mediaQueryList = window.matchMedia("(prefers-color-scheme: dark)");
  const updateTheme = () => {
    detectTheme();
  };
  mediaQueryList.addEventListener("change", updateTheme);

  onUnmounted(() => {
    mediaQueryList.removeEventListener("change", updateTheme);
  });
});
</script>

<style>
.custom-dropdown {
  position: relative;
  display: inline-block;
}

.custom-dropbtn {
  padding: 5px;
  font-size: 16px;
  border: none;
  cursor: pointer;
  border-radius: 10px;
  width: 40px;
}

.custom-dropdown-content {
  display: none;
  position: absolute;
  /* Cambiamos la posición a absoluta */
  top: 100%;
  /* Lo posicionamos debajo del botón */
  left: 0;
  /* Lo alineamos con la izquierda del botón */
  background-color: #f9f9f9;
  width: 200px;
  box-shadow: 0px 8px 16px 0px rgba(228, 180, 180, 0.2);
  z-index: 1;
  border-radius: 10px;
}

.custom-dropdown-content span {
  padding: 8px 16px;
  text-decoration: none;
  display: block;
  cursor: pointer;
}

.custom-dropdown-content span:hover {
  background-color: #f1f1f1;
  border-radius: 10px;
}

.custom-dropdown:hover .custom-dropdown-content {
  display: block;
}
</style>
