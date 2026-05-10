<template>
  <canvas ref="canvasRef" class="love-canvas"></canvas>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';

const canvasRef = ref<HTMLCanvasElement | null>(null);
let animationId: number | null = null;
let ctx: CanvasRenderingContext2D | null = null;
let particles: Particle[] = [];

interface Particle {
  x: number;
  y: number;
  size: number;
  speedX: number;
  speedY: number;
  opacity: number;
  color: string;
  rotation: number;
  rotationSpeed: number;
  type: 'heart' | 'petal';
}

const colors = ['#FF6B6B', '#FF9F43', '#FFB8B8', '#FFCCCC', '#E8B4C8', '#FF69B4', '#FFA07A'];

function createParticle(canvas: HTMLCanvasElement): Particle {
  const types: ('heart' | 'petal')[] = ['heart', 'petal'];
  return {
    x: Math.random() * canvas.width,
    y: canvas.height + Math.random() * 100,
    size: Math.random() * 15 + 8,
    speedX: (Math.random() - 0.5) * 1,
    speedY: -(Math.random() * 1.5 + 0.5),
    opacity: Math.random() * 0.6 + 0.3,
    color: colors[Math.floor(Math.random() * colors.length)],
    rotation: Math.random() * Math.PI * 2,
    rotationSpeed: (Math.random() - 0.5) * 0.02,
    type: types[Math.floor(Math.random() * types.length)]
  };
}

function drawHeart(ctx: CanvasRenderingContext2D, x: number, y: number, size: number, color: string, opacity: number) {
  ctx.save();
  ctx.translate(x, y);
  ctx.beginPath();
  ctx.fillStyle = color;
  ctx.globalAlpha = opacity;
  ctx.moveTo(0, size * 0.3);
  ctx.bezierCurveTo(-size * 0.5, -size * 0.3, -size, size * 0.1, 0, size * 0.8);
  ctx.bezierCurveTo(size, size * 0.1, size * 0.5, -size * 0.3, 0, size * 0.3);
  ctx.fill();
  ctx.restore();
}

function drawPetal(ctx: CanvasRenderingContext2D, x: number, y: number, size: number, color: string, opacity: number, rotation: number) {
  ctx.save();
  ctx.translate(x, y);
  ctx.rotate(rotation);
  ctx.beginPath();
  ctx.fillStyle = color;
  ctx.globalAlpha = opacity;
  ctx.ellipse(0, 0, size * 0.6, size, 0, 0, Math.PI * 2);
  ctx.fill();
  ctx.restore();
}

function animate() {
  if (!canvasRef.value || !ctx) return;

  const canvas = canvasRef.value;
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  particles.forEach((p, index) => {
    p.x += p.speedX;
    p.y += p.speedY;
    p.rotation += p.rotationSpeed;
    p.opacity -= 0.002;

    if (p.type === 'heart') {
      drawHeart(ctx!, p.x, p.y, p.size, p.color, p.opacity);
    } else {
      drawPetal(ctx!, p.x, p.y, p.size, p.color, p.opacity, p.rotation);
    }

    if (p.opacity <= 0 || p.y < -50) {
      particles.splice(index, 1);
      particles.push(createParticle(canvas));
    }
  });

  animationId = requestAnimationFrame(animate);
}

function resizeCanvas() {
  if (!canvasRef.value) return;
  const canvas = canvasRef.value;
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
}

onMounted(() => {
  if (!canvasRef.value) return;
  ctx = canvasRef.value.getContext('2d');
  resizeCanvas();
  window.addEventListener('resize', resizeCanvas);

  for (let i = 0; i < 30; i++) {
    const canvas = canvasRef.value;
    const p = createParticle(canvas);
    p.y = Math.random() * canvas.height;
    particles.push(p);
  }

  animate();
});

onUnmounted(() => {
  if (animationId) {
    cancelAnimationFrame(animationId);
  }
  window.removeEventListener('resize', resizeCanvas);
});
</script>

<style scoped>
.love-canvas {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  pointer-events: none;
}
</style>
