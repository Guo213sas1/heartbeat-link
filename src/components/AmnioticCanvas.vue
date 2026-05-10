<template>
  <canvas ref="canvasRef" class="amniotic-canvas"></canvas>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, watch } from 'vue';

interface Particle {
  x: number;
  y: number;
  size: number;
  speedX: number;
  speedY: number;
  opacity: number;
  hue: number;
}

interface Ripple {
  x: number;
  y: number;
  radius: number;
  maxRadius: number;
  opacity: number;
}

const props = defineProps<{
  isKicking?: boolean;
}>();

const emit = defineEmits<{
  kickTriggered: [];
}>();

const canvasRef = ref<HTMLCanvasElement | null>(null);
let animationId: number | null = null;
let ctx: CanvasRenderingContext2D | null = null;
let particles: Particle[] = [];
let ripples: Ripple[] = [];
let isKicking = false;
let kickTimeout: ReturnType<typeof setTimeout> | null = null;

const colors = {
  primary: { h: 330, s: 65, l: 78 },
  secondary: { h: 200, s: 55, l: 80 },
  accent: { h: 280, s: 40, l: 90 }
};

function createParticle(width: number, height: number): Particle {
  const colorChoice = Math.random();
  let hue: number;
  if (colorChoice < 0.4) {
    hue = colors.primary.h;
  } else if (colorChoice < 0.7) {
    hue = colors.secondary.h;
  } else {
    hue = colors.accent.h;
  }

  return {
    x: Math.random() * width,
    y: Math.random() * height,
    size: 2 + Math.random() * 6,
    speedX: (Math.random() - 0.5) * 0.5,
    speedY: (Math.random() - 0.5) * 0.3,
    opacity: 0.3 + Math.random() * 0.4,
    hue
  };
}

function initParticles(width: number, height: number) {
  particles = [];
  const particleCount = Math.floor((width * height) / 15000);
  for (let i = 0; i < particleCount; i++) {
    particles.push(createParticle(width, height));
  }
}

function createRipple(x: number, y: number) {
  ripples.push({
    x,
    y,
    radius: 0,
    maxRadius: 150 + Math.random() * 100,
    opacity: 0.5
  });
}

function drawParticle(p: Particle) {
  if (!ctx) return;
  ctx.beginPath();
  ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
  ctx.fillStyle = `hsla(${p.hue}, ${50 + p.opacity * 30}%, ${70 + p.opacity * 20}%, ${p.opacity})`;
  ctx.fill();
}

function drawRipple(r: Ripple) {
  if (!ctx) return;
  ctx.beginPath();
  ctx.arc(r.x, r.y, r.radius, 0, Math.PI * 2);
  ctx.strokeStyle = `hsla(330, 65%, 78%, ${r.opacity})`;
  ctx.lineWidth = 2;
  ctx.stroke();
}

function updateParticle(p: Particle, boost: boolean) {
  const speedMultiplier = boost ? 3 : 1;
  p.x += p.speedX * speedMultiplier;
  p.y += p.speedY * speedMultiplier;

  if (p.x < -20) p.x = canvasRef.value?.width || 0 + 20;
  if (p.x > (canvasRef.value?.width || 0) + 20) p.x = -20;
  if (p.y < -20) p.y = canvasRef.value?.height || 0 + 20;
  if (p.y > (canvasRef.value?.height || 0) + 20) p.y = -20;
}

function updateRipple(r: Ripple) {
  r.radius += 3;
  r.opacity *= 0.95;
}

function animate() {
  if (!ctx || !canvasRef.value) return;

  ctx.clearRect(0, 0, canvasRef.value.width, canvasRef.value.height);

  particles.forEach(p => {
    updateParticle(p, isKicking);
    drawParticle(p);
  });

  ripples = ripples.filter(r => r.opacity > 0.01);
  ripples.forEach(r => {
    updateRipple(r);
    drawRipple(r);
  });

  animationId = requestAnimationFrame(animate);
}

function handleResize() {
  if (!canvasRef.value) return;
  canvasRef.value.width = window.innerWidth;
  canvasRef.value.height = window.innerHeight;
  initParticles(canvasRef.value.width, canvasRef.value.height);
}

function handleKick() {
  if (!canvasRef.value) return;

  const rect = canvasRef.value.getBoundingClientRect();
  const x = Math.random() * rect.width;
  const y = rect.height * 0.4 + Math.random() * (rect.height * 0.3);

  createRipple(x, y);
  emit('kickTriggered');

  isKicking = true;
  if (kickTimeout) clearTimeout(kickTimeout);
  kickTimeout = setTimeout(() => {
    isKicking = false;
  }, 1000);
}

watch(() => props.isKicking, (newVal) => {
  if (newVal) {
    handleKick();
  }
});

onMounted(() => {
  if (!canvasRef.value) return;
  ctx = canvasRef.value.getContext('2d');
  handleResize();
  animate();
  window.addEventListener('resize', handleResize);
});

onUnmounted(() => {
  if (animationId) cancelAnimationFrame(animationId);
  window.removeEventListener('resize', handleResize);
  if (kickTimeout) clearTimeout(kickTimeout);
});

defineExpose({ triggerKick: handleKick });
</script>

<style scoped>
.amniotic-canvas {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  pointer-events: none;
}
</style>
