<script setup>
import { ref, onMounted } from 'vue'
import TiltCard from './TiltCard.vue'

const visible = ref(false)
const el = ref(null)

const projects = [
  {
    title: 'Bot Loup-Garou',
    subtitle: 'Hackathon Botpress',
    badge: { text: '1re place 🥇', class: 'bg-blue/10 text-blue' },
    description: "Bot Discord qui implémente un jeu de Loup-Garou complet dans n'importe quel serveur, développé avec le ADK de Botpress.",
    medal: true,
    images: ['/images/botpress-team.webp', ''],
    accent: 'blue',
    links: [
      { label: 'GitHub', url: 'https://github.com/dev-t0ny/bp-hacks-2026', icon: 'github' },
    ],
  },
  {
    title: 'Canopia Québec',
    subtitle: 'Application forestière',
    badge: { text: 'Hackathon · 2 jours', class: 'bg-pink/10 text-pink' },
    description: "Application de planification forestière automatisée pour le Québec. Interprétation de photos aériennes, données LIDAR et hauteur de canopée propulsées par IA.",
    images: ['/images/canopia-1.png', '/images/canopia-2.png'],
    accent: 'beige',
    links: [
      { label: 'Site web', url: 'https://canopiaquebec.ca/', icon: 'link' },
    ],
  },
  {
    title: 'WeatherMellon',
    subtitle: 'Hackathon NASA',
    badge: { text: 'Hackathon NASA', class: 'bg-pink/10 text-pink' },
    description: "Application de prédiction météorologique à long terme (10, 20, 30 ans) basée sur l'analyse des données climatiques des 10 dernières années.",
    images: ['/images/nasa-screenshot.png', '/images/nasa-team.webp'],
    accent: 'pink',
    links: [
      { label: 'Site web', url: 'https://weathermellon.info/', icon: 'link' },
      { label: 'Vidéo démo', url: 'https://youtu.be/pyqiwq4nu68', icon: 'video' },
    ],
  },
  {
    title: 'CRM Camping',
    subtitle: 'Startup',
    badge: { text: 'Startup', class: 'bg-beige/15 text-[#a08b65]' },
    description: "Solution CRM sur mesure pour la gestion de campings. Gestion des réservations, clients et opérations quotidiennes.",
    images: ['', ''],
    accent: 'beige',
    links: [
      { label: 'Camping De La Joie', url: 'https://delajoiequebec.com/', icon: 'link' },
    ],
  },
]

const accentMap = {
  blue: 'bg-blue-soft dark:bg-blue/10 text-blue/30',
  pink: 'bg-pink-soft dark:bg-pink/10 text-pink/30',
  beige: 'bg-beige-soft dark:bg-beige/10 text-beige/50',
}

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry]) => { if (entry.isIntersecting) visible.value = true },
    { threshold: 0.05 }
  )
  if (el.value) observer.observe(el.value)
})
</script>

<template>
  <section id="projets" ref="el" class="px-6 py-12">
    <div class="mx-auto max-w-[860px]">
      <h2
        class="relative mb-7 inline-block font-heading text-[22px] font-semibold tracking-tight dark:text-dark-text transition-all duration-600"
        :class="visible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-6'"
      >
        Projets
        <span class="absolute -bottom-1.5 left-0 h-[3px] w-9 rounded-full bg-pink" />
      </h2>

      <div class="flex flex-col gap-8">
        <TiltCard
          v-for="(project, i) in projects"
          :key="project.title"
        >
        <article
          class="group relative overflow-hidden rounded-2xl border border-border bg-card shadow-sm transition-all duration-300 hover:shadow-lg dark:border-dark-border dark:bg-dark-card"
          :class="visible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-6'"
          :style="{ transitionDelay: visible ? `${i * 100}ms` : '0ms' }"
        >
          <!-- Screenshot gallery -->
          <div class="grid grid-cols-2">
            <template v-for="(img, j) in project.images" :key="j">
              <div
                v-if="img"
                class="aspect-[16/9] overflow-hidden"
                :class="j === 0 ? 'border-r border-border dark:border-dark-border' : ''"
              >
                <img
                  :src="img"
                  :alt="project.title + ' screenshot ' + (j+1)"
                  class="h-full w-full object-cover transition-transform duration-500 group-hover:scale-[1.03]"
                />
              </div>
              <div
                v-else
                class="flex aspect-[16/9] items-center justify-center"
                :class="[
                  accentMap[project.accent],
                  j === 0 ? 'border-r border-border dark:border-dark-border' : '',
                ]"
              >
                <svg class="h-8 w-8 opacity-50" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1" stroke-linecap="round" stroke-linejoin="round">
                  <rect width="18" height="18" x="3" y="3" rx="2" ry="2"/><circle cx="9" cy="9" r="2"/><path d="m21 15-3.086-3.086a2 2 0 0 0-2.828 0L6 21"/>
                </svg>
              </div>
            </template>
          </div>

          <!-- Project info -->
          <div class="px-7 py-6">
            <span
              class="mb-2 inline-flex w-fit items-center rounded px-2.5 py-1 text-[11px] font-bold uppercase tracking-wider"
              :class="project.badge.class"
            >
              {{ project.badge.text }}
            </span>
            <h3 class="font-heading text-lg font-semibold tracking-tight dark:text-dark-text">
              {{ project.title }}
            </h3>
            <p class="mt-1.5 text-[14px] leading-relaxed text-text-secondary dark:text-dark-text-secondary">
              {{ project.description }}
            </p>

            <div class="mt-4 flex flex-wrap gap-2.5">
              <a
                v-for="link in project.links"
                :key="link.url"
                :href="link.url"
                target="_blank"
                class="inline-flex items-center gap-1.5 rounded-lg border border-blue/25 px-3.5 py-1.5 text-[13px] font-semibold text-blue transition-all duration-200 hover:border-blue hover:bg-blue-soft dark:hover:bg-blue/10"
              >
                <svg v-if="link.icon === 'github'" class="h-3.5 w-3.5 fill-current" viewBox="0 0 24 24"><path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/></svg>
                <svg v-if="link.icon === 'link'" class="h-3.5 w-3.5 fill-current" viewBox="0 0 24 24"><path d="M3.9 12c0-1.71 1.39-3.1 3.1-3.1h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-1.9H7c-1.71 0-3.1-1.39-3.1-3.1zM8 13h8v-2H8v2zm9-6h-4v1.9h4c1.71 0 3.1 1.39 3.1 3.1s-1.39 3.1-3.1 3.1h-4V17h4c2.76 0 5-2.24 5-5s-2.24-5-5-5z"/></svg>
                <svg v-if="link.icon === 'video'" class="h-3.5 w-3.5 fill-current" viewBox="0 0 24 24"><path d="M10 16.5l6-4.5-6-4.5v9zM12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8z"/></svg>
                {{ link.label }}
              </a>
            </div>
          </div>
        </article>
        </TiltCard>
      </div>
    </div>
  </section>
</template>
