<!-- components/Preloader.vue -->
<template>
  <div v-if="visible" class="pl-overlay" aria-hidden="false">
    <div class="pl-center">
      <div class="spinner" />
      <div class="text">Загрузка...</div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  visible: { type: Boolean, default: true }
})
</script>

<style scoped>
.pl-overlay {
  position: fixed;
  inset: 0;
  background: #0b0b0b;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 13000;
}
.pl-center {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.spinner {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  border: 5px solid rgba(255,255,255,0.08);
  border-top-color: rgba(255,255,255,0.85);
  animation: spin 1s linear infinite;
}
.text {
  color: #cfcfcf;
  margin-top: 12px;
  font-size: 14px;
}
@keyframes spin {
  to { transform: rotate(360deg); }
}
</style>
