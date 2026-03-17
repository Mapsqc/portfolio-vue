<script setup>
import { ref, onMounted } from 'vue'

const isDark = ref(false)

function applyTheme(dark) {
  document.documentElement.classList.toggle('dark', dark)
  document.body.style.background = dark ? '#141416' : '#faf9f7'
  document.body.style.color = dark ? '#e4e4e7' : '#2c2c2c'
}

function toggle() {
  isDark.value = !isDark.value
  applyTheme(isDark.value)
  localStorage.setItem('theme', isDark.value ? 'dark' : 'light')
}

onMounted(() => {
  const saved = localStorage.getItem('theme')
  if (saved === 'dark' || (!saved && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
    isDark.value = true
    applyTheme(true)
  }
})
</script>

<template>
  <button
    @click="toggle"
    class="flex h-9 w-9 items-center justify-center rounded-full border border-border bg-bg text-text-secondary transition-all duration-200 hover:border-text-light hover:text-text dark:border-dark-border dark:bg-dark-card dark:text-dark-text-secondary dark:hover:border-dark-text-secondary dark:hover:text-dark-text"
    :title="isDark ? 'Mode clair' : 'Mode sombre'"
    aria-label="Changer de thème"
  >
    <!-- Sun icon (visible in dark mode) -->
    <svg v-if="isDark" class="h-4 w-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <circle cx="12" cy="12" r="5"/><line x1="12" x2="12" y1="1" y2="3"/><line x1="12" x2="12" y1="21" y2="23"/><line x1="4.22" x2="5.64" y1="4.22" y2="5.64"/><line x1="18.36" x2="19.78" y1="18.36" y2="19.78"/><line x1="1" x2="3" y1="12" y2="12"/><line x1="21" x2="23" y1="12" y2="12"/><line x1="4.22" x2="5.64" y1="19.78" y2="18.36"/><line x1="18.36" x2="19.78" y1="5.64" y2="4.22"/>
    </svg>
    <!-- Moon icon (visible in light mode) -->
    <svg v-else class="h-4 w-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/>
    </svg>
  </button>
</template>
