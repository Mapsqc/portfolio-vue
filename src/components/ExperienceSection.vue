<script setup>
import { ref, onMounted } from 'vue'

const visible = ref(false)
const el = ref(null)

const experiences = [
  {
    title: 'Développeur Full-Stack',
    org: 'RAMQ — Gouvernement du Québec',
    desc: "Développement d'applications pour la Régie de l'assurance maladie du Québec.",
    icon: 'building',
    bgClass: 'bg-blue-soft dark:bg-blue/10',
  },
  {
    title: 'Étudiant en informatique',
    org: 'Cégep Garneau — Québec',
    desc: "Formation en développement logiciel et technologies de l'information.",
    icon: 'graduation',
    bgClass: 'bg-pink-soft dark:bg-pink/10',
  },
]

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry]) => { if (entry.isIntersecting) visible.value = true },
    { threshold: 0.1 }
  )
  if (el.value) observer.observe(el.value)
})
</script>

<template>
  <section id="experience" ref="el" class="px-6 py-12">
    <div class="mx-auto max-w-[860px]">
      <h2
        class="relative mb-7 inline-block font-heading text-[22px] font-semibold tracking-tight dark:text-dark-text transition-all duration-600"
        :class="visible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-6'"
      >
        Expérience
        <span class="absolute -bottom-1.5 left-0 h-[3px] w-9 rounded-full bg-beige" />
      </h2>

      <div class="flex flex-col gap-4">
        <div
          v-for="(exp, i) in experiences"
          :key="exp.title"

          class="flex items-start gap-5 rounded-2xl border border-border bg-card p-6 shadow-sm transition-all duration-300 hover:-translate-y-0.5 hover:shadow-md dark:border-dark-border dark:bg-dark-card"
          :class="visible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-6'"
          :style="{ transitionDelay: visible ? `${i * 80}ms` : '0ms' }"
        >
          <div
            class="flex h-12 w-12 flex-shrink-0 items-center justify-center rounded-xl"
            :class="exp.bgClass"
          >
            <svg v-if="exp.icon === 'building'" class="h-5 w-5 text-blue" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
              <rect x="4" y="2" width="16" height="20" rx="2" ry="2" /><path d="M9 22v-4h6v4" /><path d="M8 6h.01" /><path d="M16 6h.01" /><path d="M12 6h.01" /><path d="M12 10h.01" /><path d="M12 14h.01" /><path d="M16 10h.01" /><path d="M16 14h.01" /><path d="M8 10h.01" /><path d="M8 14h.01" />
            </svg>
            <svg v-if="exp.icon === 'graduation'" class="h-5 w-5 text-pink" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
              <path d="M22 10v6M2 10l10-5 10 5-10 5z" /><path d="M6 12v5c3 3 9 3 12 0v-5" />
            </svg>
          </div>
          <div>
            <h3 class="font-heading text-base font-semibold tracking-tight dark:text-dark-text">{{ exp.title }}</h3>
            <p class="text-sm font-medium text-text-secondary dark:text-dark-text-secondary">{{ exp.org }}</p>
            <p class="mt-1 text-[13px] text-text-light dark:text-dark-text-light">{{ exp.desc }}</p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
