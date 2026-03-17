<script setup>
import { ref, onMounted } from 'vue'

const visible = ref(false)
const el = ref(null)

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry]) => { if (entry.isIntersecting) visible.value = true },
    { threshold: 0.1 }
  )
  if (el.value) observer.observe(el.value)
})
</script>

<template>
  <section id="about" ref="el" class="px-6 py-14">
    <div class="mx-auto max-w-[860px]">
      <h2
        class="relative mb-8 inline-block font-heading text-[22px] font-semibold tracking-tight text-pink dark:text-pink transition-all duration-600"
        :class="visible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-6'"
      >
        À propos
        <span class="absolute -bottom-1.5 left-0 h-[3px] w-9 rounded-full bg-pink" />
      </h2>

      <div
        class="grid items-stretch gap-6 md:grid-cols-[1fr_1fr] transition-all duration-700"
        :class="visible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-6'"
      >
        <!-- Left: About text -->
        <div class="flex flex-col justify-center rounded-2xl border border-border bg-card p-8 shadow-sm dark:border-dark-border dark:bg-dark-card">
          <p class="text-[15px] leading-relaxed text-text-secondary dark:text-dark-text-secondary">
            Développeur Full-Stack passionné basé à L'Ancienne-Lorette, Québec. Je travaille au
            <span class="font-semibold text-text dark:text-dark-text">Gouvernement du Québec (RAMQ)</span>
            tout en poursuivant mes études en informatique au
            <span class="font-semibold text-text dark:text-dark-text">Cégep Garneau</span>.
          </p>
          <p class="mt-4 text-[15px] leading-relaxed text-text-secondary dark:text-dark-text-secondary">
            En parallèle, j'ai fondé ma propre startup où je développe des
            <span class="font-semibold text-text dark:text-dark-text">solutions CRM pour l'industrie du camping</span>.
            Amateur de hackathons, j'ai remporté la 1re place au Hackathon Botpress et participé au Hackathon de la NASA.
          </p>
          <p class="mt-4 text-[15px] leading-relaxed text-text-secondary dark:text-dark-text-secondary">
            <span class="font-semibold text-text dark:text-dark-text">Bilingue français-anglais</span>,
            je suis toujours à la recherche de nouveaux défis techniques et d'opportunités de collaboration.
          </p>
        </div>

        <!-- Right: Photo -->
        <div class="overflow-hidden rounded-2xl border border-border shadow-sm dark:border-dark-border min-h-[320px]">
          <img
            src="https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?w=600&h=800&fit=crop&crop=face"
            alt="Photo de profil"
            class="h-full w-full object-cover"
          />
        </div>
      </div>
    </div>
  </section>
</template>
