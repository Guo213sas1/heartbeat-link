<template>
  <div class="welcome-page" :class="{ 'is-opening': isOpening }">
    <LoveParticles />

    <div class="content-wrapper">
      <div class="gift-box" :class="{ 'is-open': isOpen }" @click="handleOpenGift">
        <div class="gift-box-inner">
          <div class="gift-ribbon-v"></div>
          <div class="gift-ribbon-h"></div>
          <div class="gift-bow"></div>
        </div>
        <div class="gift-box-lid" :class="{ 'is-open': isOpen }">
          <div class="lid-ribbon"></div>
        </div>
      </div>

      <div class="text-content" :class="{ 'is-hidden': isOpening }">
        <h1 class="main-title">母亲节快乐，亲爱的妈妈</h1>
        <p class="subtitle">这是今年最特别的礼物，也是你这一年最辛苦的成果。</p>
        <p class="sub-subtitle">—— 爱你的程序员</p>
      </div>

      <button
        class="open-button"
        :class="{ 'is-hidden': isOpening || isOpen }"
        @click="handleOpenGift"
      >
        <span class="button-text">拆开给妈妈的礼物</span>
        <span class="button-icon">🎁</span>
      </button>

      <div class="fallback-link" :class="{ 'is-hidden': isOpening }">
        <p>如果不是5月母亲节期间，<a href="javascript:void(0)" @click="goToMain">点击这里直接进入</a></p>
      </div>
    </div>

    <div class="fade-overlay" :class="{ 'is-active': showOverlay }"></div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import LoveParticles from './LoveParticles.vue';

const emit = defineEmits<{
  (e: 'enter'): void;
}>();

const isOpen = ref(false);
const isOpening = ref(false);
const showOverlay = ref(false);

function handleOpenGift() {
  if (isOpen.value || isOpening.value) return;
  isOpen.value = true;

  setTimeout(() => {
    isOpening.value = true;
    showOverlay.value = true;
  }, 800);

  setTimeout(() => {
    emit('enter');
  }, 1800);
}

function goToMain() {
  isOpening.value = true;
  showOverlay.value = true;
  setTimeout(() => {
    emit('enter');
  }, 500);
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
}

.gift-ribbon-v {
  position: absolute;
  left: 50%;
  top: 0;
  width: 16px;
  height: 100%;
  background: linear-gradient(180deg, #FFD93D 0%, #F4A300 100%);
  transform: translateX(-50%);
}

.gift-ribbon-h {
  position: absolute;
  top: 50%;
  left: 0;
  width: 100%;
  height: 16px;
  background: linear-gradient(90deg, #FFD93D 0%, #F4A300 50%, #FFD93D 100%);
  transform: translateY(-50%);
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

.fallback-link {
  margin-top: 32px;
  font-size: 13px;
  color: #A69076;
}

.fallback-link a {
  color: #FF6B6B;
  text-decoration: underline;
  cursor: pointer;
}

.fallback-link.is-hidden {
  opacity: 0;
}

.fade-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #FFF0F5 0%, #FFE8E0 100%);
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
}
</style>
