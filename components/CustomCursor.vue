<template>
  <div
    ref="cursor"
    class="custom-cursor"
    :style="{
      backgroundColor: color,
      width: size + 'px',
      height: size + 'px',
    }"
  />
</template>
<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue'

const props = defineProps({
  color: { type: String, default: '#ff3b3b' },
})

const isHoveringButton = ref(false)
const size = computed(() => (isHoveringButton.value ? 40 : 20))

const cursor = ref(null)
let mouseX = 0,
  mouseY = 0,
  curX = 0,
  curY = 0,
  frameId
const EASE = 0.15

function moveHandler(e) {
  mouseX = e.clientX
  mouseY = e.clientY

  const el = document.elementFromPoint(mouseX, mouseY)
  if (
    el &&
    (el.tagName === 'BUTTON' ||
      el.tagName === 'A' ||
      el.closest('.cursor-big'))
  ) {
    isHoveringButton.value = true
  } else {
    isHoveringButton.value = false
  }
}

function animate() {
  curX += (mouseX - curX) * EASE
  curY += (mouseY - curY) * EASE
  if (cursor.value) {
    const currentSize = size.value
    cursor.value.style.width = currentSize + 'px'
    cursor.value.style.height = currentSize + 'px'
    cursor.value.style.transform = `translate3d(${curX - currentSize / 2}px, ${
      curY - currentSize / 2
    }px, 0)`
  }
  frameId = requestAnimationFrame(animate)
}

onMounted(() => {
  document.body.style.cursor = 'none'
  window.addEventListener('mousemove', moveHandler)
  animate()
})

onUnmounted(() => {
  window.removeEventListener('mousemove', moveHandler)
  cancelAnimationFrame(frameId)
  document.body.style.cursor = ''
})
</script>


<style scoped>
.custom-cursor {
  position: fixed;
  top: 0;
  left: 0;
  border-radius: 50%;
  pointer-events: none;
  opacity: 0.9;
  box-shadow: 0 0 25px 1px rgba(255, 255, 255, 0.7);
  z-index: 9999;
  mix-blend-mode: screen;
  will-change: transform, width, height;
  transition:
    transform 0.07s ease-out,
    width 0.3s ease,
    height 0.3s ease,
    background-color 0.25s ease,
    opacity 0.25s ease;
}
</style>