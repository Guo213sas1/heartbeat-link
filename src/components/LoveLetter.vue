<template>
  <div class="love-letter" :class="{ 'is-exiting': isExiting }">
    <div class="letter-bg"></div>

    <div class="floating-hearts">
      <div
        v-for="i in 20"
        :key="i"
        class="heart"
        :style="getHeartStyle(i)"
      >
        ❤️
      </div>
    </div>

    <div class="floating-petals">
      <div
        v-for="i in 15"
        :key="'petal-' + i"
        class="petal"
        :style="getPetalStyle(i)"
      >
        🌸
      </div>
    </div>

    <div class="letter-content" :class="{ 'is-visible': showContent }">
      <div class="envelope" :class="{ 'is-open': isEnvelopeOpen }">
        <div class="envelope-front">
          <div class="envelope-ribbon"></div>
        </div>
        <div class="envelope-back"></div>
      </div>

      <div class="letter-paper" :class="{ 'is-open': isEnvelopeOpen }">
        <div class="letter-content-inner">
          <div class="heart-icon">💕</div>
          <h2 class="letter-title">给我的宝贝老婆</h2>
          <div class="letter-body">
            <p
              v-for="(line, index) in letterLines"
              :key="index"
              class="letter-line"
              :class="{ 'is-visible': visibleLines >= index }"
              :style="{ transitionDelay: `${index * 0.3}s` }"
            >
              {{ line }}
            </p>
          </div>
          <div class="letter-signature" :class="{ 'is-visible': showSignature }">
            <p>永远爱你的人</p>
            <p class="signature-name">—— 爱你的郭助</p>
          </div>
        </div>
      </div>

      <button
        v-if="showEnterButton"
        class="enter-button"
        @click="handleEnter"
      >
        <span>接收这份爱意 💝</span>
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

const emit = defineEmits<{
  (e: 'enter'): void;
}>();

const isEnvelopeOpen = ref(false);
const showContent = ref(false);
const visibleLines = ref(-1);
const showSignature = ref(false);
const showEnterButton = ref(false);
const isExiting = ref(false);

const letterLines = [
  '谢谢你选择了我',
  '谢谢你愿意为我承受这一切',
  '280个日夜，你辛苦了',
  '每一次孕吐、每一声叹息、每一个难眠的夜晚',
  '都在告诉我，你有多勇敢',
  '现在，让我们一起期待',
  '那个最珍贵的见面'
];

function getHeartStyle(index: number) {
  const size = Math.random() * 20 + 10;
  const left = Math.random() * 100;
  const delay = Math.random() * 5;
  const duration = Math.random() * 3 + 4;

  return {
    left: `${left}%`,
    fontSize: `${size}px`,
    animationDelay: `${delay}s`,
    animationDuration: `${duration}s`,
    opacity: Math.random() * 0.5 + 0.3
  };
}

function getPetalStyle(index: number) {
  const size = Math.random() * 15 + 10;
  const left = Math.random() * 100;
  const delay = Math.random() * 8;
  const duration = Math.random() * 5 + 5;

  return {
    left: `${left}%`,
    fontSize: `${size}px`,
    animationDelay: `${delay}s`,
    animationDuration: `${duration}s`,
    opacity: Math.random() * 0.4 + 0.2
  };
}

function handleEnter() {
  isExiting.value = true;
  setTimeout(() => {
    emit('enter');
  }, 1000);
}

onMounted(() => {
  setTimeout(() => {
    showContent.value = true;
  }, 300);

  setTimeout(() => {
    isEnvelopeOpen.value = true;
  }, 1200);

  setTimeout(() => {
    visibleLines.value = 0;
  }, 2000);

  letterLines.forEach((_, index) => {
    setTimeout(() => {
      visibleLines.value = index;
    }, 2500 + index * 400);
  });

  setTimeout(() => {
    showSignature.value = true;
  }, 2500 + letterLines.length * 400 + 500);

  setTimeout(() => {
    showEnterButton.value = true;
  }, 2500 + letterLines.length * 400 + 1500);
});
</script>

<style scoped>
.love-letter {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #1a1a2e 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9998;
  overflow: hidden;
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.love-letter.is-exiting {
  opacity: 0;
  transform: scale(1.1);
}

.letter-bg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background:
    radial-gradient(circle at 20% 80%, rgba(255, 107, 107, 0.1) 0%, transparent 50%),
    radial-gradient(circle at 80% 20%, rgba(255, 159, 67, 0.1) 0%, transparent 50%),
    radial-gradient(circle at 50% 50%, rgba(255, 107, 107, 0.05) 0%, transparent 70%);
}

.floating-hearts,
.floating-petals {
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
  bottom: -50px;
  animation: floatUp linear infinite;
  color: #ff6b6b;
}

@keyframes floatUp {
  0% {
    transform: translateY(0) rotate(0deg) scale(1);
    opacity: 0;
  }
  10% {
    opacity: 1;
  }
  90% {
    opacity: 1;
  }
  100% {
    transform: translateY(-110vh) rotate(360deg) scale(0.5);
    opacity: 0;
  }
}

.petal {
  position: absolute;
  bottom: -50px;
  animation: floatUpPetal linear infinite;
  color: #ffb7c5;
}

@keyframes floatUpPetal {
  0% {
    transform: translateY(0) translateX(0) rotate(0deg);
    opacity: 0;
  }
  10% {
    opacity: 1;
  }
  50% {
    transform: translateY(-50vh) translateX(30px) rotate(180deg);
  }
  90% {
    opacity: 1;
  }
  100% {
    transform: translateY(-110vh) translateX(-30px) rotate(360deg);
    opacity: 0;
  }
}

.letter-content {
  position: relative;
  z-index: 1;
  text-align: center;
  padding: 24px;
  max-width: 500px;
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.letter-content.is-visible {
  opacity: 1;
  transform: translateY(0);
}

.envelope {
  position: relative;
  width: 180px;
  height: 120px;
  margin: 0 auto 30px;
  perspective: 1000px;
}

.envelope-front {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 80px;
  background: linear-gradient(145deg, #ffd93d 0%, #f4a300 100%);
  border-radius: 8px;
  z-index: 2;
  transition: transform 0.6s ease;
  transform-origin: bottom center;
}

.envelope.is-open .envelope-front {
  transform: rotateX(-180deg);
}

.envelope-ribbon {
  position: absolute;
  left: 50%;
  top: 0;
  width: 20px;
  height: 100%;
  background: linear-gradient(180deg, #ff6b6b 0%, #ff9f43 100%);
  transform: translateX(-50%);
}

.envelope-back {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 80px;
  background: linear-gradient(145deg, #f4a300 0%, #e69500 100%);
  border-radius: 8px;
  z-index: 1;
}

.letter-paper {
  position: absolute;
  top: 50%;
  left: 50%;
  width: calc(100% - 40px);
  max-width: 400px;
  background: linear-gradient(180deg, #fff9e6 0%, #fff5d6 100%);
  border-radius: 8px;
  padding: 30px 20px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
  transform: translate(-50%, -50%) scale(0.8);
  opacity: 0;
  transition: transform 0.8s ease 0.5s, opacity 0.8s ease 0.5s;
  z-index: 3;
}

.letter-paper.is-open {
  transform: translate(-50%, -50%) scale(1);
  opacity: 1;
}

.letter-content-inner {
  color: #5d4037;
}

.heart-icon {
  font-size: 40px;
  margin-bottom: 16px;
  animation: heartbeat 1.5s ease infinite;
}

@keyframes heartbeat {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.1); }
}

.letter-title {
  font-size: 24px;
  font-weight: 700;
  color: #ff6b6b;
  margin-bottom: 20px;
}

.letter-body {
  text-align: left;
  margin-bottom: 24px;
}

.letter-line {
  font-size: 16px;
  line-height: 1.8;
  color: #5d4037;
  margin-bottom: 8px;
  opacity: 0;
  transform: translateY(10px);
  transition: opacity 0.5s ease, transform 0.5s ease;
}

.letter-line.is-visible {
  opacity: 1;
  transform: translateY(0);
}

.letter-signature {
  text-align: right;
  opacity: 0;
  transform: translateY(10px);
  transition: opacity 0.5s ease, transform 0.5s ease;
}

.letter-signature.is-visible {
  opacity: 1;
  transform: translateY(0);
}

.letter-signature p {
  font-size: 14px;
  color: #8b7355;
  margin: 0;
}

.signature-name {
  font-size: 16px !important;
  font-weight: 600;
  color: #ff6b6b !important;
  margin-top: 8px !important;
}

.enter-button {
  margin-top: 30px;
  padding: 18px 40px;
  background: linear-gradient(135deg, #ff6b6b 0%, #ff9f43 100%);
  border: none;
  border-radius: 50px;
  color: white;
  font-size: 18px;
  font-weight: 600;
  cursor: pointer;
  box-shadow: 0 8px 30px rgba(255, 107, 107, 0.4);
  transition: all 0.3s ease;
  animation: pulse 2s ease infinite;
}

.enter-button:hover {
  transform: translateY(-3px) scale(1.05);
  box-shadow: 0 12px 40px rgba(255, 107, 107, 0.5);
}

.enter-button:active {
  transform: translateY(0) scale(0.98);
}

@keyframes pulse {
  0%, 100% {
    box-shadow: 0 8px 30px rgba(255, 107, 107, 0.4);
  }
  50% {
    box-shadow: 0 8px 50px rgba(255, 107, 107, 0.6);
  }
}

@media (min-width: 768px) {
  .letter-title {
    font-size: 28px;
  }

  .letter-line {
    font-size: 18px;
  }

  .envelope {
    width: 220px;
    height: 150px;
  }

  .envelope-front {
    height: 100px;
  }

  .envelope-back {
    height: 100px;
  }
}
</style>
