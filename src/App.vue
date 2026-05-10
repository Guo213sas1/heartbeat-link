<template>
  <div class="app" @click="handleAppClick">
    <AmnioticCanvas ref="canvasRef" :isKicking="isKicking" @kick-triggered="handleKickTriggered" />

    <div class="content-wrapper">
      <header class="header">
        <h1 class="title">Heartbeat Link</h1>
        <p class="subtitle">❤️ 程序员远程开发</p>
      </header>

      <main class="main-content">
        <div v-if="!dueDate" class="setup-card">
          <div class="setup-icon">👶</div>
          <h2 class="setup-title">欢迎使用 Heartbeat Link</h2>
          <p class="setup-description">请设置预产期开始追踪孕期进度</p>
          <div class="setup-form">
            <input
              type="date"
              v-model="inputDate"
              class="date-input"
              :min="minDate"
              :max="maxDate"
            />
            <button @click="setDueDate" class="start-button">开始追踪</button>
          </div>
          <p class="setup-hint">或者在 URL 中添加 ?due=YYYY-MM-DD</p>
        </div>

        <template v-else>
          <PregnancyCountdown
            :daysLeft="status.daysLeft"
            :hoursLeft="status.hoursLeft"
            :currentWeek="status.currentWeek"
            :daysIntoWeek="status.daysIntoWeek"
            :isOverdue="status.isOverdue"
            :isReady="status.isReady"
          />

          <FruitComparison
            :fruit="status.currentData.fruit"
            :fruitEmoji="status.currentData.fruitEmoji"
            :length="status.currentData.length"
            :weight="status.currentData.weight"
          />

          <DailyTip :tip="status.currentData.tip" :isNew="isTipNew" />

          <div class="interaction-hint" v-if="showHint">
            <span class="hint-icon">👆</span>
            <span class="hint-text">点击屏幕任何地方感受胎动</span>
          </div>
        </template>
      </main>

      <footer class="footer" v-if="dueDate">
        <p class="footer-message">—— 爱你的程序员</p>
        <p class="footer-sub">远程开发 · 母亲节献礼</p>
      </footer>
    </div>

    <HeartbeatSimulator v-if="dueDate" />
    <KickFeedback :trigger="kickTriggered" />
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, watch } from 'vue';
import AmnioticCanvas from './components/AmnioticCanvas.vue';
import PregnancyCountdown from './components/PregnancyCountdown.vue';
import FruitComparison from './components/FruitComparison.vue';
import DailyTip from './components/DailyTip.vue';
import HeartbeatSimulator from './components/HeartbeatSimulator.vue';
import KickFeedback from './components/KickFeedback.vue';
import { getBabyStatus } from './data/fruit_data';

const dueDate = ref<string | null>(null);
const inputDate = ref('');
const isKicking = ref(false);
const kickTriggered = ref(false);
const showHint = ref(true);
const isTipNew = ref(true);
const canvasRef = ref<InstanceType<typeof AmnioticCanvas> | null>(null);

const today = new Date();
const minDate = new Date(today.getTime() - 280 * 24 * 60 * 60 * 1000).toISOString().split('T')[0];
const maxDate = new Date(today.getTime() + 365 * 24 * 60 * 60 * 1000).toISOString().split('T')[0];

const status = computed(() => {
  if (!dueDate.value) {
    return {
      daysLeft: 0,
      hoursLeft: 0,
      currentWeek: 0,
      daysIntoWeek: 0,
      currentData: {
        week: 0,
        fruit: '',
        fruitEmoji: '',
        weight: '',
        length: '',
        tip: ''
      },
      isOverdue: false,
      isReady: false
    };
  }
  return getBabyStatus(dueDate.value);
});

function parseUrlDate(): string | null {
  const params = new URLSearchParams(window.location.search);
  return params.get('due');
}

function setDueDateFromUrl() {
  const urlDate = parseUrlDate();
  if (urlDate) {
    const date = new Date(urlDate);
    if (!isNaN(date.getTime())) {
      dueDate.value = urlDate;
      localStorage.setItem('heartbeat-link-due-date', urlDate);
      return true;
    }
  }
  return false;
}

function setDueDate() {
  if (!inputDate.value) return;
  dueDate.value = inputDate.value;
  localStorage.setItem('heartbeat-link-due-date', inputDate.value);
  window.history.replaceState({}, '', window.location.pathname);
}

function handleAppClick(e: MouseEvent) {
  if (!dueDate.value) return;

  const target = e.target as HTMLElement;
  if (target.closest('.heartbeat-simulator') || target.closest('button') || target.closest('input')) {
    return;
  }

  triggerKick();
}

function triggerKick() {
  if (isKicking.value) return;

  isKicking.value = true;
  kickTriggered.value = true;
  showHint.value = false;

  canvasRef.value?.triggerKick();

  setTimeout(() => {
    isKicking.value = false;
  }, 1000);

  setTimeout(() => {
    kickTriggered.value = false;
  }, 3500);
}

function handleKickTriggered() {
  if (navigator.vibrate) {
    navigator.vibrate(100);
  }
}

onMounted(() => {
  const savedDate = localStorage.getItem('heartbeat-link-due-date');
  if (savedDate) {
    dueDate.value = savedDate;
  } else {
    setDueDateFromUrl();
  }

  if (dueDate.value) {
    const envDate = import.meta.env.VITE_DUE_DATE;
    if (envDate && !savedDate) {
      dueDate.value = envDate;
    }
  }
});

watch(dueDate, () => {
  if (dueDate.value) {
    setTimeout(() => {
      isTipNew.value = false;
    }, 3000);
  }
});
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

:root {
  --color-primary: #E8B4C8;
  --color-secondary: #B4D4E8;
  --color-accent: #F6E6F0;
  --color-bg: #FDF5F7;
  --color-text: #5D4E60;
  --color-text-secondary: #8B7B8E;
  --color-heart: #FF6B8A;
}

body {
  font-family: 'Noto Sans SC', system-ui, sans-serif;
  background: linear-gradient(135deg, var(--color-bg) 0%, #EBF4FC 100%);
  min-height: 100vh;
  color: var(--color-text);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  min-height: 100vh;
}
</style>

<style scoped>
.app {
  min-height: 100vh;
  position: relative;
  overflow-x: hidden;
}

.content-wrapper {
  position: relative;
  z-index: 1;
  max-width: 480px;
  margin: 0 auto;
  padding: 24px 16px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.header {
  text-align: center;
  padding: 24px 0;
  animation: fadeInDown 0.8s ease-out;
}

@keyframes fadeInDown {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.title {
  font-family: 'Quicksand', 'Noto Sans SC', sans-serif;
  font-size: 32px;
  font-weight: 700;
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-secondary) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-bottom: 8px;
}

.subtitle {
  font-size: 14px;
  color: var(--color-text-secondary);
  font-weight: 400;
}

.main-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.setup-card {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.9) 0%, rgba(248, 240, 245, 0.9) 100%);
  border-radius: 24px;
  padding: 40px 24px;
  text-align: center;
  box-shadow: 0 8px 32px rgba(232, 180, 200, 0.2);
}

.setup-icon {
  font-size: 64px;
  margin-bottom: 16px;
  display: block;
}

.setup-title {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 22px;
  color: var(--color-text);
  margin-bottom: 12px;
  font-weight: 600;
}

.setup-description {
  font-size: 15px;
  color: var(--color-text-secondary);
  margin-bottom: 24px;
}

.setup-form {
  display: flex;
  flex-direction: column;
  gap: 16px;
  margin-bottom: 16px;
}

.date-input {
  width: 100%;
  padding: 14px 20px;
  border: 2px solid rgba(232, 180, 200, 0.3);
  border-radius: 12px;
  font-family: 'Quicksand', 'Noto Sans SC', sans-serif;
  font-size: 16px;
  color: var(--color-text);
  background: rgba(255, 255, 255, 0.8);
  outline: none;
  transition: border-color 0.3s ease;
}

.date-input:focus {
  border-color: var(--color-primary);
}

.start-button {
  width: 100%;
  padding: 16px 24px;
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-secondary) 100%);
  border: none;
  border-radius: 12px;
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 16px;
  font-weight: 600;
  color: white;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.start-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(232, 180, 200, 0.4);
}

.start-button:active {
  transform: translateY(0);
}

.setup-hint {
  font-size: 12px;
  color: var(--color-text-secondary);
  opacity: 0.7;
}

.interaction-hint {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 12px;
  background: rgba(255, 255, 255, 0.6);
  border-radius: 20px;
  animation: fadeIn 1s ease-out 2s both;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

.hint-icon {
  font-size: 18px;
  animation: wave 1s ease-in-out infinite;
}

@keyframes wave {
  0%, 100% { transform: rotate(-10deg); }
  50% { transform: rotate(10deg); }
}

.hint-text {
  font-size: 13px;
  color: var(--color-text-secondary);
}

.footer {
  text-align: center;
  padding: 32px 0 16px;
  margin-top: auto;
}

.footer-message {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 16px;
  color: var(--color-text);
  font-style: italic;
  margin-bottom: 4px;
}

.footer-sub {
  font-size: 12px;
  color: var(--color-text-secondary);
  opacity: 0.7;
}

@media (min-width: 768px) {
  .content-wrapper {
    padding: 48px 24px;
  }

  .title {
    font-size: 40px;
  }

  .setup-card {
    padding: 48px 32px;
  }
}
</style>
