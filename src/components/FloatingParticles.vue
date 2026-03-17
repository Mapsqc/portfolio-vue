<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const canvas = ref(null)
let animationId = null

const STAR_COUNT = 18
const MAX_TWINKLE = 28

// Shooting star sizes: small, medium, large
const SIZES = [
  { thickness: [0.6, 1], length: [20, 40], speed: [2, 3.5], opacity: [0.15, 0.22] },   // small
  { thickness: [1.2, 2], length: [40, 70], speed: [1.5, 3], opacity: [0.2, 0.3] },      // medium
  { thickness: [2.2, 3.5], length: [60, 110], speed: [1, 2.5], opacity: [0.25, 0.4] },  // large
]

function randRange(min, max) { return min + Math.random() * (max - min) }

class ShootingStar {
  constructor(w, h, colors) {
    this.colors = colors
    this.w = w
    this.h = h
    this.reset(true)
  }

  reset(initial = false) {
    // Pick size category: 50% small, 30% medium, 20% large
    const r = Math.random()
    const size = r < 0.5 ? SIZES[0] : r < 0.8 ? SIZES[1] : SIZES[2]

    const side = Math.random()
    if (side < 0.5) {
      this.x = -20
      this.y = Math.random() * this.h * 0.8
      this.angle = -20 + Math.random() * 40
    } else {
      this.x = this.w + 20
      this.y = Math.random() * this.h * 0.8
      this.angle = 140 + Math.random() * 40
    }

    const rad = (this.angle * Math.PI) / 180
    const speed = randRange(size.speed[0], size.speed[1])
    this.vx = Math.cos(rad) * speed
    this.vy = Math.sin(rad) * (0.2 + Math.random() * 0.4) + 0.15

    this.length = randRange(size.length[0], size.length[1])
    this.opacity = 0
    this.maxOpacity = randRange(size.opacity[0], size.opacity[1])
    this.color = this.colors[Math.floor(Math.random() * this.colors.length)]
    this.thickness = randRange(size.thickness[0], size.thickness[1])

    this.life = 0
    this.maxLife = 180 + Math.random() * 280

    this.delay = initial ? Math.random() * 500 : Math.random() * 120
    this.active = false
  }

  update() {
    if (this.delay > 0) { this.delay--; return }
    this.active = true
    this.life++

    const progress = this.life / this.maxLife
    if (progress < 0.12) {
      this.opacity = this.maxOpacity * (progress / 0.12)
    } else if (progress > 0.8) {
      this.opacity = this.maxOpacity * ((1 - progress) / 0.2)
    } else {
      this.opacity = this.maxOpacity
    }

    this.x += this.vx
    this.y += this.vy

    if (this.life >= this.maxLife || this.x < -150 || this.x > this.w + 150 || this.y > this.h + 50) {
      this.reset()
    }
  }

  draw(ctx) {
    if (!this.active || this.opacity <= 0) return

    ctx.save()
    ctx.globalAlpha = this.opacity

    const mag = Math.sqrt(this.vx * this.vx + this.vy * this.vy)
    const tailX = this.x - (this.vx / mag) * this.length
    const tailY = this.y - (this.vy / mag) * this.length

    const gradient = ctx.createLinearGradient(tailX, tailY, this.x, this.y)
    gradient.addColorStop(0, 'transparent')
    gradient.addColorStop(0.7, this.color)
    gradient.addColorStop(1, this.color)

    ctx.beginPath()
    ctx.moveTo(tailX, tailY)
    ctx.lineTo(this.x, this.y)
    ctx.strokeStyle = gradient
    ctx.lineWidth = this.thickness
    ctx.lineCap = 'round'
    ctx.stroke()

    // Glow at head
    ctx.beginPath()
    ctx.arc(this.x, this.y, this.thickness * 1.2, 0, Math.PI * 2)
    ctx.fillStyle = this.color
    ctx.globalAlpha = this.opacity * 0.7
    ctx.fill()

    ctx.restore()
  }
}

class TwinkleStar {
  constructor(w, h, colors) {
    this.colors = colors
    this.w = w
    this.h = h
    this.reset(true)
  }

  reset(initial = false) {
    this.x = Math.random() * this.w
    this.y = Math.random() * this.h
    this.size = 1 + Math.random() * 2.5
    this.color = this.colors[Math.floor(Math.random() * this.colors.length)]
    this.opacity = 0
    this.maxOpacity = 0.3 + Math.random() * 0.3
    this.life = 0
    this.maxLife = 100 + Math.random() * 200
    this.delay = initial ? Math.random() * 100 : 20 + Math.random() * 80
    this.active = false
  }

  update() {
    if (this.delay > 0) { this.delay--; return }
    this.active = true
    this.life++

    const progress = this.life / this.maxLife
    // Pulse: fade in, bright, fade out
    if (progress < 0.3) {
      this.opacity = this.maxOpacity * (progress / 0.3)
    } else if (progress > 0.7) {
      this.opacity = this.maxOpacity * ((1 - progress) / 0.3)
    } else {
      // Twinkle/pulse effect
      this.opacity = this.maxOpacity * (0.85 + Math.sin(this.life * 0.3) * 0.15)
    }

    if (this.life >= this.maxLife) {
      this.reset()
    }
  }

  draw(ctx) {
    if (!this.active || this.opacity <= 0) return

    ctx.save()
    ctx.globalAlpha = this.opacity

    // 4-point star shape
    const s = this.size
    ctx.translate(this.x, this.y)
    ctx.fillStyle = this.color
    ctx.beginPath()
    ctx.moveTo(0, -s * 1.8)
    ctx.quadraticCurveTo(s * 0.3, -s * 0.3, s * 1.8, 0)
    ctx.quadraticCurveTo(s * 0.3, s * 0.3, 0, s * 1.8)
    ctx.quadraticCurveTo(-s * 0.3, s * 0.3, -s * 1.8, 0)
    ctx.quadraticCurveTo(-s * 0.3, -s * 0.3, 0, -s * 1.8)
    ctx.fill()

    // Center glow
    ctx.beginPath()
    ctx.arc(0, 0, s * 0.5, 0, Math.PI * 2)
    ctx.globalAlpha = this.opacity * 0.5
    ctx.fill()

    ctx.restore()
  }
}

function getColors() {
  const isDark = document.documentElement.classList.contains('dark')
  if (isDark) {
    return [
      'rgba(107, 155, 210, 0.9)',
      'rgba(212, 160, 176, 0.8)',
      'rgba(212, 197, 169, 0.8)',
      'rgba(160, 190, 230, 0.7)',
    ]
  }
  return [
    'rgba(107, 155, 210, 0.7)',
    'rgba(212, 160, 176, 0.55)',
    'rgba(212, 197, 169, 0.55)',
    'rgba(140, 175, 215, 0.5)',
  ]
}

let stars = []
let twinkles = []

function init() {
  const c = canvas.value
  if (!c) return
  const ctx = c.getContext('2d')

  function resize() {
    c.width = window.innerWidth
    c.height = window.innerHeight
  }
  resize()
  window.addEventListener('resize', resize, { passive: true })

  const colors = getColors()
  stars = []
  twinkles = []
  for (let i = 0; i < STAR_COUNT; i++) {
    stars.push(new ShootingStar(c.width, c.height, colors))
  }
  for (let i = 0; i < MAX_TWINKLE; i++) {
    twinkles.push(new TwinkleStar(c.width, c.height, colors))
  }

  function animate() {
    ctx.clearRect(0, 0, c.width, c.height)
    for (const s of stars) {
      s.w = c.width
      s.h = c.height
      s.update()
      s.draw(ctx)
    }
    for (const t of twinkles) {
      t.w = c.width
      t.h = c.height
      t.update()
      t.draw(ctx)
    }
    animationId = requestAnimationFrame(animate)
  }
  animate()

  const observer = new MutationObserver(() => {
    const newColors = getColors()
    for (const s of stars) {
      s.colors = newColors
      s.color = newColors[Math.floor(Math.random() * newColors.length)]
    }
    for (const t of twinkles) {
      t.colors = newColors
      t.color = newColors[Math.floor(Math.random() * newColors.length)]
    }
  })
  observer.observe(document.documentElement, { attributes: true, attributeFilter: ['class'] })
}

onMounted(() => init())
onUnmounted(() => cancelAnimationFrame(animationId))
</script>

<template>
  <canvas
    ref="canvas"
    class="pointer-events-none fixed inset-0 z-0"
  />
</template>
