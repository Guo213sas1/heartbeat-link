<template>
  <div class="love-animation" :class="{ 'is-exiting': isExiting }">
    <div class="animation-bg"></div>

    <div class="floating-hearts">
      <div
        v-for="i in 30"
        :key="i"
        class="heart"
        :style="getHeartStyle(i)"
      >
        ❤️
      </div>
    </div>

    <div class="floating-petals">
      <div
        v-for="i in 20"
        :key="'petal-' + i"
        class="petal"
        :style="getPetalStyle(i)"
      >
        🌸
      </div>
    </div>

    <div class="floating-stars">
      <div
        v-for="i in 15"
        :key="'star-' + i"
        class="star"
        :style="getStarStyle(i)"
      >
        ✨
      </div>
    </div>

    <div class="center-content">
      <div class="sparkle-ring">
        <div class="ring-inner"></div>
        <div class="ring-outer"></div>
      </div>
      <div class="gift-emoji">🎁</div>
      <div class="animation-text">宝贝，你的礼物来啦</div>
      <div class="progress-bar">
        <div class="progress-fill"></div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

const emit = defineEmits<{
  (e: 'enter'): void;
}>();

const isExiting = ref(false);

function getHeartStyle(index: number) {
  const size = Math.random() * 25 + 12;
  const left = Math.random() * 100;
  const delay = Math.random() * 4;
  const duration = Math.random() * 4 + 3;
  const rotation = Math.random() * 360;

  return {
    left: `${left}%`,
    fontSize: `${size}px`,
    animationDelay: `${delay}s`,
    animationDuration: `${duration}s`,
    opacity: Math.random() * 0.6 + 0.4,
    transform: `rotate(${rotation}deg)`
  };
}

function getPetalStyle(index: number) {
  const size = Math.random() * 18 + 12;
  const left = Math.random() * 100;
  const delay = Math.random() * 6;
  const duration = Math.random() * 6 + 4;

  return {
    left: `${left}%`,
    fontSize: `${size}px`,
    animationDelay: `${delay}s`,
    animationDuration: `${duration}s`,
    opacity: Math.random() * 0.5 + 0.3
  };
}

function getStarStyle(index: number) {
  const size = Math.random() * 16 + 8;
  const left = Math.random() * 100;
  const top = Math.random() * 100;
  const delay = Math.random() * 3;
  const duration = Math.random() * 2 + 1.5;

  return {
    left: `${left}%`,
    top: `${top}%`,
    fontSize: `${size}px`,
    animationDelay: `${delay}s`,
    animationDuration: `${duration}s`,
    opacity: Math.random() * 0.7 + 0.3
  };
}

onMounted(() => {
  setTimeout(() => {
    isExiting.value = true;
    setTimeout(() => {
      emit('enter');
    }, 800);
  }, 3000);
});
</script>

<style scoped>
.love-animation {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #FFF0F5 0%, #FFE4E1 30%, #FFF8E7 60%, #FFEFD5 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9998;
  overflow: hidden;
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.love-animation.is-exiting {
  opacity: 0;
  transform: scale(1.05);
}

.animation-bg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background:
    radial-gradient(circle at 30% 20%, rgba(255, 182, 193, 0.3) 0%, transparent 40%),
    radial-gradient(circle at 70% 80%, rgba(255, 218, 185, 0.3) 0%, transparent 40%),
    radial-gradient(circle at 50% 50%, rgba(255, 182, 193, 0.2) 0%, transparent 60%);
}

.floating-hearts,
.floating-petals,
.floating-stars {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  overflow: hidden;
}

.heart {
  position: absolute;
  bottom: -60px;
  animation: floatUpHeart ease-in-out infinite;
}

@keyframes floatUpHeart {
  0% {
    transform: translateY(0) rotate(0deg) scale(1);
    opacity: 0;
  }
  10% {
    opacity: 1;
  }
  50% {
    transform: translateY(-50vh) rotate(180deg) scale(1.2);
  }
  90% {
    opacity: 1;
  }
  100% {
    transform: translateY(-110vh) rotate(360deg) scale(0.6);
    opacity: 0;
  }
}

.petal {
  position: absolute;
  bottom: -60px;
  animation: floatUpPetal ease-in-out infinite;
}

@keyframes floatUpPetal {
  0% {
    transform: translateY(0) translateX(0) rotate(0deg);
    opacity: 0;
  }
  10% {
    opacity: 1;
  }
  30% {
    transform: translateY(-30vh) translateX(40px) rotate(90deg);
  }
  60% {
    transform: translateY(-60vh) translateX(-20px) rotate(180deg);
  }
  90% {
    opacity: 1;
  }
  100% {
    transform: translateY(-110vh) translateX(30px) rotate(360deg);
    opacity: 0;
  }
}

.star {
  position: absolute;
  animation: twinkle ease-in-out infinite;
}

@keyframes twinkle {
  0%, 100% {
    opacity: 0.3;
    transform: scale(1);
  }
  50% {
    opacity: 1;
    transform: scale(1.3);
  }
}

.center-content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 24px;
}

.sparkle-ring {
  position: relative;
  width: 180px;
  height: 180px;
  animation: ringPulse 2s ease-in-out infinite;
}

@keyframes ringPulse {
  0%, 100% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.1);
    opacity: 0.8;
  }
}

.ring-inner {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 120px;
  height: 120px;
  border: 3px solid rgba(255, 107, 107, 0.4);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  animation: ringRotate 3s linear infinite;
}

.ring-outer {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 160px;
  height: 160px;
  border: 2px solid rgba(255, 159, 67, 0.3);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  animation: ringRotate 4s linear infinite reverse;
}

@keyframes ringRotate {
  from {
    transform: translate(-50%, -50%) rotate(0deg);
  }
  to {
    transform: translate(-50%, -50%) rotate(360deg);
  }
}

.gift-emoji {
  font-size: 80px;
  animation: giftBounce 1.5s ease-in-out infinite;
  filter: drop-shadow(0 8px 20px rgba(255, 107, 107, 0.3));
}

@keyframes giftBounce {
  0%, 100% {
    transform: translateY(0) scale(1);
  }
  25% {
    transform: translateY(-10px) scale(1.05);
  }
  50% {
    transform: translateY(0) scale(1);
  }
  75% {
    transform: translateY(-5px) scale(1.02);
  }
}

.animation-text {
  font-size: 24px;
  font-weight: 600;
  color: #5D4037;
  text-shadow: 0 2px 10px rgba(255, 107, 107, 0.2);
  animation: textFade 3s ease-in-out infinite;
}

@keyframes textFade {
  0%, 100% {
    opacity: 0.7;
  }
  50% {
    opacity: 1;
  }
}

.progress-bar {
  width: 200px;
  height: 8px;
  background: rgba(255, 182, 193, 0.3);
  border-radius: 4px;
  overflow: hidden;
}

.progress-fill {
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #FF6B6B, #FF9F43);
  border-radius: 4px;
  animation: progressMove 3s ease-in-out infinite;
}

@keyframes progressMove {
  0% {
    width: 0%;
  }
  100% {
    width: 100%;
  }
}

@media (min-width: 768px) {
  .gift-emoji {
    font-size: 100px;
  }

  .animation-text {
    font-size: 28px;
  }

  .progress-bar {
    width: 280px;
    height: 10px;
  }

  .sparkle-ring {
    width: 220px;
    height: 220px;
  }

  .ring-inner {
    width: 150px;
    height: 150px;
  }

  .ring-outer {
    width: 200px;
    height: 200px;
  }
}
</style>
