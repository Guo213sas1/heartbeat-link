<template>
  <div class="video-transition" :class="{ 'is-exiting': isExiting }">
    <div class="video-container">
      <video
        ref="videoRef"
        class="gift-video"
        muted
        playsinline
        webkit-playsinline
        x5-playsinline
        preload="auto"
        @loadedmetadata="handleVideoLoaded"
        @canplay="handleCanPlay"
        @ended="handleVideoEnded"
        @play="handlePlay"
        @pause="handlePause"
        @error="handleVideoError"
        @waiting="handleWaiting"
      >
        <source :src="videoPath" type="video/mp4" />
        您的浏览器不支持视频播放
      </video>
      
      <div class="video-overlay">
        <div class="video-content">
          <div class="play-icon" v-if="showPlayButton && !isLoading" @click="playVideo">
            <span class="play-arrow">▶</span>
          </div>
          <div class="video-text" v-if="!isPlaying && !isLoading && showPlayButton">
            <p>点击播放礼物视频</p>
          </div>
          <div class="loading-text" v-if="isLoading">
            <div class="spinner"></div>
            <p>🎬 正在加载...</p>
          </div>
          <div class="error-text" v-if="hasError">
            <p>😢 视频加载失败</p>
            <p class="error-hint">请检查网络连接</p>
          </div>
        </div>
        
        <div class="progress-info" v-if="isPlaying && duration > 0">
          <span class="current-time">{{ formatTime(currentTime) }}</span>
          <span class="time-separator">/</span>
          <span class="total-time">{{ formatTime(duration) }}</span>
        </div>
        
        <div class="progress-bar-container" v-if="duration > 0">
          <div class="progress-bar" :style="{ width: progressPercent + '%' }"></div>
        </div>
      </div>
    </div>
    
    <div class="floating-hearts">
      <div
        v-for="i in 15"
        :key="i"
        class="heart"
        :style="getHeartStyle(i)"
      >
        ❤️
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from 'vue';

const emit = defineEmits<{
  (e: 'enter'): void;
}>();

const videoPath = 'video/4f50fefb8938579d3dd8e55db283ee95.mp4';

const videoRef = ref<HTMLVideoElement | null>(null);
const isExiting = ref(false);
const isPlaying = ref(false);
const isLoading = ref(true);
const showPlayButton = ref(true);
const hasError = ref(false);
const currentTime = ref(0);
const duration = ref(0);

const progressPercent = computed(() => {
  if (duration.value === 0) return 0;
  return (currentTime.value / duration.value) * 100;
});

function getHeartStyle(index: number) {
  const size = Math.random() * 20 + 10;
  const left = Math.random() * 100;
  const delay = Math.random() * 3;
  const dur = Math.random() * 4 + 3;

  return {
    left: `${left}%`,
    fontSize: `${size}px`,
    animationDelay: `${delay}s`,
    animationDuration: `${dur}s`,
    opacity: Math.random() * 0.5 + 0.3
  };
}

function formatTime(seconds: number): string {
  const mins = Math.floor(seconds / 60);
  const secs = Math.floor(seconds % 60);
  return `${mins}:${secs.toString().padStart(2, '0')}`;
}

function handleVideoLoaded() {
  if (videoRef.value) {
    duration.value = videoRef.value.duration;
  }
}

function handleCanPlay() {
  isLoading.value = false;
}

function handleWaiting() {
  isLoading.value = true;
}

function handlePlay() {
  isPlaying.value = true;
  isLoading.value = false;
}

function handlePause() {
  isPlaying.value = false;
}

function handleVideoError(e: Event) {
  console.error('Video error:', e);
  isLoading.value = false;
  hasError.value = true;
}

async function playVideo() {
  if (!videoRef.value) return;
  
  showPlayButton.value = false;
  hasError.value = false;
  
  try {
    await videoRef.value.play();
    isPlaying.value = true;
  } catch (error) {
    console.error('Play failed:', error);
    showPlayButton.value = true;
    hasError.value = true;
  }
}

function handleVideoEnded() {
  isExiting.value = true;
  setTimeout(() => {
    emit('enter');
  }, 800);
}

onMounted(() => {
  setInterval(() => {
    if (videoRef.value && isPlaying.value) {
      currentTime.value = videoRef.value.currentTime;
    }
  }, 100);
});
</script>

<style scoped>
.video-transition {
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

.video-transition.is-exiting {
  opacity: 0;
  transform: scale(1.05);
}

.video-container {
  position: relative;
  width: 100%;
  max-width: 640px;
  height: 100%;
  max-height: 480px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.gift-video {
  width: 100%;
  height: 100%;
  object-fit: contain;
  border-radius: 16px;
  box-shadow: 0 20px 60px rgba(255, 107, 107, 0.3);
  background: #000;
}

.video-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: rgba(0, 0, 0, 0);
}

.video-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.play-icon {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  background: rgba(255, 107, 107, 0.9);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  animation: pulse 2s ease-in-out infinite;
  box-shadow: 0 8px 30px rgba(255, 107, 107, 0.5);
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
    box-shadow: 0 8px 30px rgba(255, 107, 107, 0.5);
  }
  50% {
    transform: scale(1.1);
    box-shadow: 0 12px 40px rgba(255, 107, 107, 0.7);
  }
}

.play-arrow {
  color: white;
  font-size: 28px;
  margin-left: 6px;
}

.video-text, .loading-text, .error-text {
  margin-top: 20px;
  color: #5D4037;
  font-size: 16px;
  font-weight: 500;
  text-align: center;
}

.error-hint {
  font-size: 12px;
  color: #8B7355;
  margin-top: 8px;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid rgba(255, 107, 107, 0.3);
  border-top-color: #FF6B6B;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 12px;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

.progress-info {
  position: absolute;
  bottom: 60px;
  right: 30px;
  color: white;
  font-size: 14px;
  text-shadow: 0 1px 3px rgba(0, 0, 0, 0.5);
  display: flex;
  gap: 4px;
}

.progress-bar-container {
  position: absolute;
  bottom: 30px;
  left: 30px;
  right: 30px;
  height: 4px;
  background: rgba(255, 255, 255, 0.3);
  border-radius: 2px;
  overflow: hidden;
}

.progress-bar {
  height: 100%;
  background: linear-gradient(90deg, #FF6B6B, #FF9F43);
  border-radius: 2px;
  transition: width 0.1s linear;
}

.floating-hearts {
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
  90% {
    opacity: 1;
  }
  100% {
    transform: translateY(-110vh) rotate(360deg) scale(0.6);
    opacity: 0;
  }
}

@media (min-width: 768px) {
  .play-icon {
    width: 100px;
    height: 100px;
  }

  .play-arrow {
    font-size: 36px;
    margin-left: 8px;
  }

  .video-text, .loading-text, .error-text {
    font-size: 18px;
  }

  .progress-bar-container {
    height: 6px;
  }
}
</style>