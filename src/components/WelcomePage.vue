<template>
  <div class="welcome-page" :class="{ 'is-opening': isOpening }">
    <LoveParticles />

    <div class="content-wrapper">
      <div class="gift-box" :class="{ 'is-open': isOpen }" @click="handleOpenGift">
        <div class="gift-box-inner">
          <div class="gift-ribbon-v"></div>
          <div class="gift-ribbon-h"></div>
          <div class="gift-bow"></div>
          
          <div class="baby-reveal" :class="{ 'is-visible': showBaby }">
            <span class="baby-emoji">👶</span>
          </div>
        </div>
        <div class="gift-box-lid" :class="{ 'is-open': isOpen }">
          <div class="lid-ribbon"></div>
        </div>
      </div>

      <div class="text-content" :class="{ 'is-hidden': isOpening }">
        <h1 class="main-title">母亲节快乐，偶滴宝</h1>
        <p class="subtitle">这是今年最特别的礼物，也是你这一年最辛苦的成果。</p>
        <p class="sub-subtitle">—— 爱你的郭助</p>
      </div>

      <button
        class="open-button"
        :class="{ 'is-hidden': isOpening || isOpen }"
        @click="handleOpenGift"
      >
        <span class="button-text">拆开给妈妈的礼物</span>
        <span class="button-icon">🎁</span>
      </button>
    </div>

    <div class="magic-particles">
      <span
        v-for="particle in particles"
        :key="particle.id"
        class="magic-particle"
        :style="{
          left: particle.x + 'px',
          animationDelay: particle.delay + 's',
          fontSize: particle.size + 'px'
        }"
      >{{ particle.emoji }}</span>
    </div>

    <div class="celebration-screen" :class="{ 'is-visible': showCelebration }">
      <div class="celebration-content">
        <div class="celebration-baby">👶</div>
        <div class="celebration-hearts">
          <span class="heart">❤️</span>
          <span class="heart">🧡</span>
          <span class="heart">💛</span>
        </div>
        <p class="celebration-text">送给最棒的妈妈！</p>
        <p class="celebration-sub">愿你和宝宝健康快乐</p>
      </div>
    </div>

    <div class="fade-overlay" :class="{ 'is-active': showOverlay }"></div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue';
import LoveParticles from './LoveParticles.vue';

const emit = defineEmits<{
  (e: 'enter'): void;
}>();

const isOpen = ref(false);
const isOpening = ref(false);
const showOverlay = ref(false);
const showBaby = ref(false);
const showCelebration = ref(false);

const emojis = ['❤️', '💛', '💚', '💙', '💜', '🌟', '✨', '🎉', '🎊', '🎁', '🎈', '🍼', '🐰', '🦋', '🌸', '🌺', '🌈'];

const particles = reactive<{ id: number; x: number; delay: number; size: number; emoji: string }[]>([]);

function generateParticles() {
  particles.length = 0;
  for (let i = 0; i < 20; i++) {
    particles.push({
      id: i,
      x: Math.random() * window.innerWidth,
      delay: Math.random() * 0.5,
      size: Math.random() * 20 + 20,
      emoji: emojis[Math.floor(Math.random() * emojis.length)]
    });
  }
}

function handleOpenGift() {
  if (isOpen.value || isOpening.value) return;
  
  generateParticles();
  isOpen.value = true;

  setTimeout(() => {
    showBaby.value = true;
  }, 500);

  setTimeout(() => {
    showCelebration.value = true;
    isOpening.value = true;
  }, 1200);

  setTimeout(() => {
    showOverlay.value = true;
  }, 2000);

  setTimeout(() => {
    emit('enter');
  }, 3500);
}
</script>

<style scoped>
.welcome-page {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #FFF0F5 0%, #FFF8E7 50%, #FFF0F5 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  overflow: hidden;
}

.content-wrapper {
  position: relative;
  z-index: 1;
  text-align: center;
  padding: 24px;
  max-width: 480px;
  display: flex;
  flex-direction: column;
  align-items: center;
  transition: opacity 0.5s ease, transform 0.5s ease;
}

.content-wrapper.is-hidden {
  opacity: 0;
  transform: translateY(-30px);
}

.gift-box {
  position: relative;
  width: 120px;
  height: 100px;
  cursor: pointer;
  margin-bottom: 48px;
  transform-origin: center bottom;
  transition: transform 0.3s ease;
}

.gift-box:hover {
  transform: scale(1.05);
}

.gift-box.is-open {
  animation: giftBounce 0.5s ease;
}

@keyframes giftBounce {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.1); }
}

.gift-box-inner {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 70px;
  background: linear-gradient(145deg, #FF9F43 0%, #FF6B6B 100%);
  border-radius: 8px;
  box-shadow: 0 8px 24px rgba(255, 107, 107, 0.3);
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.gift-ribbon-v {
  position: absolute;
  left: 50%;
  top: 0;
  width: 16px;
  height: 100%;
  background: linear-gradient(180deg, #FFD93D 0%, #F4A300 100%);
  transform: translateX(-50%);
  z-index: 2;
}

.gift-ribbon-h {
  position: absolute;
  top: 50%;
  left: 0;
  width: 100%;
  height: 16px;
  background: linear-gradient(90deg, #FFD93D 0%, #F4A300 50%, #FFD93D 100%);
  transform: translateY(-50%);
  z-index: 2;
}

.gift-bow {
  position: absolute;
  top: -20px;
  left: 50%;
  transform: translateX(-50%);
  width: 40px;
  height: 30px;
  background: radial-gradient(ellipse at center, #FFD93D 0%, #F4A300 100%);
  border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%;
  z-index: 3;
}

.gift-bow::before,
.gift-bow::after {
  content: '';
  position: absolute;
  width: 25px;
  height: 20px;
  background: linear-gradient(135deg, #FFD93D 0%, #F4A300 100%);
  border-radius: 50%;
  top: 5px;
}

.gift-bow::before {
  left: -12px;
  transform: rotate(-30deg);
}

.gift-bow::after {
  right: -12px;
  transform: rotate(30deg);
}

.gift-box-lid {
  position: absolute;
  top: 0;
  left: -5px;
  width: calc(100% + 10px);
  height: 35px;
  background: linear-gradient(145deg, #FF9F43 0%, #FF6B6B 100%);
  border-radius: 8px 8px 4px 4px;
  transform-origin: left bottom;
  transition: transform 0.6s ease;
  box-shadow: 0 4px 12px rgba(255, 107, 107, 0.2);
  z-index: 4;
}

.gift-box-lid.is-open {
  transform: rotateX(-120deg);
}

.lid-ribbon {
  position: absolute;
  left: 50%;
  top: 0;
  width: 16px;
  height: 100%;
  background: linear-gradient(180deg, #FFD93D 0%, #F4A300 100%);
  transform: translateX(-50%);
}

.baby-reveal {
  opacity: 0;
  transform: scale(0) translateY(20px);
  transition: all 0.5s ease;
  z-index: 1;
}

.baby-reveal.is-visible {
  opacity: 1;
  transform: scale(1) translateY(0);
  animation: babyBounce 0.5s ease infinite alternate;
}

@keyframes babyBounce {
  from { transform: scale(1) translateY(0); }
  to { transform: scale(1.1) translateY(-5px); }
}

.baby-emoji {
  font-size: 40px;
}

.text-content {
  margin-bottom: 40px;
  transition: opacity 0.5s ease, transform 0.5s ease;
}

.main-title {
  font-size: 32px;
  font-weight: 700;
  color: #5D4037;
  margin-bottom: 16px;
  line-height: 1.3;
  text-shadow: 0 2px 8px rgba(255, 107, 107, 0.15);
}

.subtitle {
  font-size: 16px;
  color: #8B7355;
  line-height: 1.6;
  margin-bottom: 12px;
}

.sub-subtitle {
  font-size: 14px;
  color: #A69076;
  font-style: italic;
}

.open-button {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  padding: 18px 36px;
  background: linear-gradient(135deg, #FF6B6B 0%, #FF9F43 100%);
  border: none;
  border-radius: 50px;
  color: white;
  font-size: 18px;
  font-weight: 600;
  cursor: pointer;
  box-shadow: 0 8px 24px rgba(255, 107, 107, 0.4);
  transition: all 0.3s ease;
}

.open-button:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 32px rgba(255, 107, 107, 0.5);
}

.open-button:active {
  transform: translateY(0);
}

.open-button.is-hidden {
  opacity: 0;
  transform: translateY(20px);
  pointer-events: none;
}

.button-icon {
  font-size: 20px;
}

.magic-particles {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 5;
  overflow: hidden;
}

.magic-particle {
  position: absolute;
  bottom: -50px;
  animation: floatUp 2s ease-out forwards;
  opacity: 0;
}

@keyframes floatUp {
  0% {
    opacity: 1;
    transform: translateY(0) rotate(0deg) scale(1);
  }
  50% {
    opacity: 1;
  }
  100% {
    opacity: 0;
    transform: translateY(-100vh) rotate(360deg) scale(0.5);
  }
}

.celebration-screen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(255, 240, 245, 0.95);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  pointer-events: none;
  z-index: 10;
  transition: opacity 0.5s ease;
}

.celebration-screen.is-visible {
  opacity: 1;
  pointer-events: auto;
}

.celebration-content {
  text-align: center;
  animation: celebrationScale 0.5s ease;
}

@keyframes celebrationScale {
  from {
    transform: scale(0.5);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}

.celebration-baby {
  font-size: 120px;
  animation: babyFloat 1s ease-in-out infinite;
  margin-bottom: 24px;
}

@keyframes babyFloat {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-20px); }
}

.celebration-hearts {
  display: flex;
  justify-content: center;
  gap: 16px;
  margin-bottom: 24px;
}

.heart {
  font-size: 32px;
  animation: heartFloat 0.8s ease-in-out infinite;
}

.heart:nth-child(1) { animation-delay: 0s; }
.heart:nth-child(2) { animation-delay: 0.2s; }
.heart:nth-child(3) { animation-delay: 0.4s; }

@keyframes heartFloat {
  0%, 100% { transform: translateY(0) scale(1); }
  50% { transform: translateY(-15px) scale(1.2); }
}

.celebration-text {
  font-size: 28px;
  font-weight: 700;
  color: #5D4037;
  margin-bottom: 8px;
  animation: textFadeIn 0.5s ease 0.3s both;
}

.celebration-sub {
  font-size: 18px;
  color: #8B7355;
  animation: textFadeIn 0.5s ease 0.5s both;
}

@keyframes textFadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.fade-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #FFF0F5 0%, #FDF5F7 100%);
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.8s ease;
  z-index: 100;
}

.fade-overlay.is-active {
  opacity: 1;
}

@media (min-width: 768px) {
  .main-title {
    font-size: 40px;
  }

  .subtitle {
    font-size: 18px;
  }

  .gift-box {
    width: 150px;
    height: 130px;
  }

  .gift-box-inner {
    height: 90px;
  }

  .gift-box-lid {
    height: 45px;
  }

  .baby-emoji {
    font-size: 50px;
  }

  .celebration-baby {
    font-size: 150px;
  }

  .celebration-text {
    font-size: 36px;
  }

  .celebration-sub {
    font-size: 22px;
  }
}
</style>
