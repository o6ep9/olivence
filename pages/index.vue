<script setup>
import { ref, onMounted } from 'vue'
import PaletteDropdown from '~/components/PaletteDropdown.vue'
import CustomCursor from '~/components/CustomCursor.vue'

const selectedColor = ref('')

const pastelColors = [
  '#A8D5BA', '#F7CAC9', '#FFE5B4',
  '#B0E0E6', '#D7BDE2', '#F9E79F', '#AED6F1'
]

function getRandomPastelColor() {
  return pastelColors[Math.floor(Math.random() * pastelColors.length)]
}

function applySelectionColor(color) {
  document.documentElement.style.setProperty('--selection-color', color)
}

onMounted(() => {
  selectedColor.value = getRandomPastelColor()
  applySelectionColor(selectedColor.value)
})


watch(selectedColor, (color) => {
  applySelectionColor(color)
})
</script>

<template>
  <header class="header">
    <PaletteDropdown
      :colors="pastelColors"
      v-model="selectedColor"
    />
  </header>

  <div class="fullscreen-div">
    <div class="placeholder">
      <h2>Leinwand</h2>
      <p class="hint">Hier wird Ihre Leinwand sein..</p>
    </div>
  </div>

  <CustomCursor :color="selectedColor" />
</template>


<style>
::selection {
  background: var(--selection-color, rgba(251, 245, 132, 0.5));
  color: rgba(20, 19, 17, 0.5);
}
::-moz-selection {
  background: var(--selection-color, rgba(251, 245, 132, 0.5));
  color: rgba(20, 19, 17, 0.5);
}

:root { --bg: #121212; --panel: #1e1e1e; }


/* шапка */
.header {
  position: fixed;
  top: 0;
  left: 0;
  height: 50px;
  width: 100%;
  background: #222;
  display: flex;
  align-items: center;
  padding: 0 16px;
  z-index: 1100;
}

/* большой див под шапкой */
.fullscreen-div {
  position: fixed;
  top: 48px;
  left: 0;
  width: 100vw;
  height: calc(100vh - 48px);
  background: var(--panel);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}
.placeholder { text-align:center; color: #ddd }
.placeholder h2 { margin: 0 0 6px 0 }
.hint { margin: 0; opacity: 0.7 }

</style>
