<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import ThemeToggle from './ThemeToggle.vue'

const scrolled = ref(false)
const menuOpen = ref(false)

function onScroll() {
  scrolled.value = window.scrollY > 10
}

function scrollTo(id) {
  menuOpen.value = false
  document.getElementById(id)?.scrollIntoView({ behavior: 'smooth', block: 'start' })
}

onMounted(() => window.addEventListener('scroll', onScroll))
onUnmounted(() => window.removeEventListener('scroll', onScroll))
</script>

<template>
  <nav
    class="nav-bar fixed top-0 left-0 right-0 z-50 border-b border-border backdrop-blur-xl transition-all duration-300 dark:border-dark-border"
    :class="[scrolled ? 'shadow-sm' : '']"
  >
    <div class="mx-auto flex h-14 max-w-[860px] items-center justify-between px-6">
      <a href="#" class="font-heading text-lg font-bold tracking-tight text-text dark:text-dark-text">
        A<span class="text-blue">.</span>
      </a>

      <div class="flex items-center gap-6">
        <ul class="hidden gap-7 md:flex">
          <li v-for="item in [
            { id: 'about', label: 'À propos' },
            { id: 'projets', label: 'Projets' },
            { id: 'experience', label: 'Expérience' },
          ]" :key="item.id">
            <a
              href="#"
              class="group relative text-sm font-medium text-text-secondary transition-colors hover:text-text dark:text-dark-text-secondary dark:hover:text-dark-text"
              @click.prevent="scrollTo(item.id)"
            >
              {{ item.label }}
              <span class="absolute -bottom-1 left-0 h-0.5 w-0 rounded-full bg-blue transition-all duration-300 group-hover:w-full" />
            </a>
          </li>
        </ul>

        <ThemeToggle />

        <button
          class="flex flex-col gap-[5px] p-2 md:hidden"
          @click="menuOpen = !menuOpen"
          aria-label="Menu"
        >
          <span class="block h-0.5 w-[22px] rounded-full bg-text transition-all dark:bg-dark-text" />
          <span class="block h-0.5 w-[22px] rounded-full bg-text transition-all dark:bg-dark-text" />
          <span class="block h-0.5 w-[22px] rounded-full bg-text transition-all dark:bg-dark-text" />
        </button>
      </div>
    </div>

    <div
      v-if="menuOpen"
      class="nav-menu flex flex-col gap-4 border-b border-border px-6 py-4 backdrop-blur-xl md:hidden dark:border-dark-border"
    >
      <a
        v-for="item in [
          { id: 'competences', label: 'Compétences' },
          { id: 'projets', label: 'Projets' },
          { id: 'experience', label: 'Expérience' },
        ]"
        :key="item.id"
        href="#"
        class="text-sm font-medium text-text-secondary transition-colors hover:text-text dark:text-dark-text-secondary dark:hover:text-dark-text"
        @click.prevent="scrollTo(item.id)"
      >
        {{ item.label }}
      </a>
    </div>
  </nav>
</template>

<style scoped>
.nav-bar {
  background: rgba(250, 249, 247, 0.85);
}
:where(.dark) .nav-bar {
  background: rgba(20, 20, 22, 0.85);
}
.nav-menu {
  background: rgba(250, 249, 247, 0.97);
}
:where(.dark) .nav-menu {
  background: rgba(20, 20, 22, 0.97);
}
</style>
