<template>
  <div class="palette-wrapper">
    <button ref="btn" class="palette-btn" @click="toggle">
      Farben
    </button>

    <div
      v-if="open"
      class="color-palette"
      :style="{ top: `${paletteTop}px`, left: `${paletteLeft}px` }"
    >
      <button
        v-for="c in colors"
        :key="c"
        class="swatch"
        :style="{ backgroundColor: c, outline: c === selectedColor ? '2px solid #fff' : 'none' }"
        @click="select(c)"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, watch, nextTick } from 'vue'

const props = defineProps({
  colors: { type: Array, default: () => [] },
  modelValue: { type: String, default: '' }
})
const emit = defineEmits(['update:modelValue'])

const open = ref(false)
const selectedColor = ref(props.modelValue)

const btn = ref(null)
const paletteTop = ref(0)
const paletteLeft = ref(0)

watch(() => props.modelValue, (v) => {
  selectedColor.value = v
})

function toggle() {
  open.value = !open.value
  if (open.value) {
    nextTick(() => {
      const rect = btn.value.getBoundingClientRect()
      paletteTop.value = rect.bottom + 8 
      paletteLeft.value = rect.left
    })
  }
}

function select(color) {
  selectedColor.value = color
  emit('update:modelValue', color)
  open.value = false
}
</script>

<style scoped>
.palette-wrapper {
  display: inline-block;
}
.palette-btn {
  font-size: 14px;
  display: inline-block;
  outline: 0;
  border: 0;
  cursor: pointer;
  will-change: box-shadow, transform;
  background: radial-gradient(100% 100% at 100% 0%, #dedede 0%, #606060 100%);
  box-shadow: 0px 0.01em 0.01em rgb(45 35 66 / 40%), 
              0px 0.3em 0.7em -0.01em rgb(45 35 66 / 30%), 
              inset 0px -0.01em 0px rgb(58 65 111 / 50%);
  padding: 8px 14px;
  border-radius: 0.3em;
  color: #fff;
  height: 2.6em;
  text-shadow: 0 1px 0 rgb(0 0 0 / 40%);
  position: fixed;
  top: 8px;
  left: 16px;
  z-index: 1101;
  transition: box-shadow 0.15s ease, transform 0.15s ease;
  user-select: none;
}

.palette-btn:hover {
  box-shadow: 0px 0.1em 0.2em rgb(45 35 66 / 40%), 
              0px 0.4em 0.7em -0.1em rgb(45 35 66 / 30%), 
              inset 0px -0.1em 0px #434343;
  transform: translateY(-0.1em);
}

.palette-btn:active {
  box-shadow: inset 0px 0.1em 0.6em #434343;
  transform: translateY(0em);
}


.color-palette {
  position: fixed;
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  background: #606060;
  padding: 10px;
  border-radius: 4px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.3);
  z-index: 1200;
}

.swatch {
  width: 30px;
  height: 30px;
  border-radius: 6px;
  border: none;
  cursor: pointer;
}
</style>
