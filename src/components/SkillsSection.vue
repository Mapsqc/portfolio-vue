<script setup>
import { ref, onMounted } from 'vue'

const visible = ref(false)
const el = ref(null)

const skills = [
  { name: 'C# / .NET', cat: 'lang' },
  { name: 'TypeScript', cat: 'lang' },
  { name: 'JavaScript', cat: 'lang' },
  { name: 'Python', cat: 'lang' },
  { name: 'PL/SQL', cat: 'lang' },
  { name: 'HTML / CSS / SCSS', cat: 'lang' },
  { name: 'Vue.js', cat: 'framework' },
  { name: 'Nuxt 3', cat: 'framework' },
  { name: 'Angular', cat: 'framework' },
  { name: 'GitHub', cat: 'tool' },
  { name: 'Claude Code', cat: 'tool' },
]

const catColors = {
  lang: { dot: 'bg-blue', border: 'border-blue/30 hover:border-blue hover:bg-blue-soft dark:hover:bg-blue/10' },
  framework: { dot: 'bg-beige', border: 'border-beige/30 hover:border-beige hover:bg-beige-soft dark:hover:bg-beige/10' },
  tool: { dot: 'bg-pink', border: 'border-pink/30 hover:border-pink hover:bg-pink-soft dark:hover:bg-pink/10' },
}

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry]) => { if (entry.isIntersecting) visible.value = true },
    { threshold: 0.1 }
  )
  if (el.value) observer.observe(el.value)
})
</script>

<template>
  <section id="competences" ref="el" class="px-6 py-12">
    <div class="mx-auto max-w-[860px]">
      <h2
        class="relative mb-6 inline-block font-heading text-[22px] font-semibold tracking-tight dark:text-dark-text transition-all duration-600"
        :class="visible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-6'"
      >
        Compétences
        <span class="absolute -bottom-1.5 left-0 h-[3px] w-9 rounded-full bg-blue" />
      </h2>

      <div class="flex flex-wrap gap-2.5">
        <span
          v-for="(skill, i) in skills"
          :key="skill.name"
          class="inline-flex cursor-default items-center gap-2 rounded-full border bg-card px-[18px] py-2.5 text-sm font-medium hover:-translate-y-0.5 hover:shadow-md dark:bg-dark-card dark:text-dark-text"
          :class="[
            catColors[skill.cat].border,
            visible ? 'opacity-100 translate-y-0 skill-visible' : 'opacity-0 translate-y-4 skill-entering'
          ]"
          :style="{ '--delay': `${i * 60}ms` }"
        >
          <span class="h-2 w-2 rounded-full" :class="catColors[skill.cat].dot" />
          {{ skill.name }}
        </span>
      </div>
    </div>
  </section>
</template>

<style scoped>
.skill-entering {
  transition: opacity 0.5s ease var(--delay), transform 0.5s ease var(--delay);
}
.skill-visible {
  transition: transform 0.2s ease, box-shadow 0.2s ease, opacity 0.5s ease;
}
</style>
