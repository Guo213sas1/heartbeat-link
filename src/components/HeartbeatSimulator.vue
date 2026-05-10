<template>
  <div class="heartbeat-simulator" @click="handleClick">
    <div class="heart-container" :class="{ active: isPlaying }">
      <svg class="heart-icon" viewBox="0 0 24 24" :class="{ beating: isPlaying }">
        <path
          d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"
          fill="url(#heartGradient)"
        />
        <defs>
          <linearGradient id="heartGradient" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" stop-color="#FF6B8A" />
            <stop offset="100%" stop-color="#E8B4C8" />
          </linearGradient>
        </defs>
      </svg>
      <div class="ripple" v-if="showRipple"></div>
      <div class="ripple delay" v-if="showRipple"></div>
    </div>
    <span class="heartbeat-label">点击听心跳</span>
    <span class="heartbeat-rate" v-if="isPlaying">{{ currentRate }} bpm</span>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';

const emit = defineEmits<{
  heartbeatPlaying: [isPlaying: boolean];
}>();

const isPlaying = ref(false);
const showRipple = ref(false);
const currentRate = ref(140);

let audioContext: AudioContext | null = null;
let heartbeatInterval: ReturnType<typeof setInterval> | null = null;
let rateChangeTimeout: ReturnType<typeof setTimeout> | null = null;

const rates = [120, 130, 140, 150, 160];

function createHeartbeatSound() {
  if (!audioContext) {
    audioContext = new (window.AudioContext || (window as any).webkitAudioContext)();
  }

  const oscillator = audioContext.createOscillator();
  const gainNode = audioContext.createGain();

  oscillator.connect(gainNode);
  gainNode.connect(audioContext.destination);

  oscillator.frequency.value = 80;
  oscillator.type = 'sine';

  gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
  gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.15);

  oscillator.start(audioContext.currentTime);
  oscillator.stop(audioContext.currentTime + 0.15);

  setTimeout(() => {
    if (!audioContext) return;
    const osc2 = audioContext.createOscillator();
    const gain2 = audioContext.createGain();
    osc2.connect(gain2);
    gain2.connect(audioContext.destination);
    osc2.frequency.value = 60;
    osc2.type = 'sine';
    gain2.gain.setValueAtTime(0.2, audioContext.currentTime);
    gain2.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.1);
    osc2.start(audioContext.currentTime);
    osc2.stop(audioContext.currentTime + 0.1);
  }, 150);
}

function startHeartbeat() {
  if (isPlaying.value) return;

  isPlaying.value = true;
  showRipple.value = true;
  emit('heartbeatPlaying', true);

  currentRate.value = rates[Math.floor(Math.random() * rates.length)];

  const beatInterval = 60000 / currentRate.value;
  createHeartbeatSound();
  heartbeatInterval = setInterval(createHeartbeatSound, beatInterval);

  rateChangeTimeout = setTimeout(() => {
    if (isPlaying.value) {
      currentRate.value = rates[Math.floor(Math.random() * rates.length)];
      if (heartbeatInterval) {
        clearInterval(heartbeatInterval);
        const newInterval = 60000 / currentRate.value;
        heartbeatInterval = setInterval(createHeartbeatSound, newInterval);
      }
      startHeartbeat();
    }
  }, 5000 + Math.random() * 5000);
}

function stopHeartbeat() {
  isPlaying.value = false;
  showRipple.value = false;
  emit('heartbeatPlaying', false);

  if (heartbeatInterval) {
    clearInterval(heartbeatInterval);
    heartbeatInterval = null;
  }
  if (rateChangeTimeout) {
    clearTimeout(rateChangeTimeout);
    rateChangeTimeout = null;
  }
}

function handleClick() {
  if (isPlaying.value) {
    stopHeartbeat();
  } else {
    startHeartbeat();
  }
}

onUnmounted(() => {
  stopHeartbeat();
  if (audioContext) {
    audioContext.close();
  }
});
</script>

<style scoped>
.heartbeat-simulator {
  position: fixed;
  bottom: 24px;
  left: 24px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  z-index: 100;
  cursor: pointer;
}

.heart-container {
  position: relative;
  width: 56px;
  height: 56px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.heart-icon {
  width: 40px;
  height: 40px;
  filter: drop-shadow(0 2px 8px rgba(255, 107, 138, 0.4));
  transition: transform 0.2s ease;
}

.heart-icon.beating {
  animation: heartbeatBeat 0.15s ease-in-out;
}

@keyframes heartbeatBeat {
  0% { transform: scale(1); }
  50% { transform: scale(1.3); }
  100% { transform: scale(1); }
}

.heartbeat-label {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 11px;
  color: #8B7B8E;
  white-space: nowrap;
}

.heartbeat-rate {
  font-family: 'Quicksand', sans-serif;
  font-size: 12px;
  color: #FF6B8A;
  font-weight: 600;
}

.ripple {
  position: absolute;
  width: 56px;
  height: 56px;
  border: 2px solid #FF6B8A;
  border-radius: 50%;
  animation: rippleEffect 1.5s ease-out infinite;
  opacity: 0;
}

.ripple.delay {
  animation-delay: 0.75s;
}

@keyframes rippleEffect {
  0% {
    transform: scale(1);
    opacity: 0.6;
  }
  100% {
    transform: scale(2);
    opacity: 0;
  }
}
</style>
