<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import VanillaTilt from 'vanilla-tilt'

const card = ref(null)

onMounted(() => {
  if (card.value) {
    VanillaTilt.init(card.value, {
      max: 3,
      speed: 600,
      glare: true,
      'max-glare': 0.04,
      scale: 1.005,
      perspective: 1500,
    })
  }
})

onUnmounted(() => {
  if (card.value?.vanillaTilt) {
    card.value.vanillaTilt.destroy()
  }
})
</script>

<template>
  <div ref="card" class="tilt-card">
    <slot />
  </div>
</template>

<style scoped>
.tilt-card {
  transform-style: preserve-3d;
}
</style>
