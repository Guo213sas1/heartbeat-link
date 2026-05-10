<template>
  <div class="daily-tip" :class="{ highlight: isNew }">
    <div class="tip-header">
      <span class="tip-icon">💝</span>
      <span class="tip-label">每日贴士</span>
    </div>
    <p class="tip-content">{{ tip }}</p>
    <div class="tip-footer">
      <span class="heart-decoration">❤️</span>
      <span class="footer-text">妈妈辛苦了</span>
      <span class="heart-decoration">❤️</span>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  tip: string;
  isNew?: boolean;
}>();
</script>

<style scoped>
.daily-tip {
  background: linear-gradient(135deg, rgba(246, 230, 240, 0.9) 0%, rgba(232, 180, 200, 0.3) 100%);
  border-radius: 20px;
  padding: 20px;
  border-left: 4px solid #E8B4C8;
}

.daily-tip.highlight {
  animation: tipGlow 2s ease-in-out infinite;
}

@keyframes tipGlow {
  0%, 100% { box-shadow: 0 0 0 rgba(232, 180, 200, 0); }
  50% { box-shadow: 0 0 20px rgba(232, 180, 200, 0.4); }
}

.tip-header {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 12px;
}

.tip-icon {
  font-size: 18px;
}

.tip-label {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 14px;
  color: #E8B4C8;
  font-weight: 600;
}

.tip-content {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 15px;
  color: #5D4E60;
  line-height: 1.7;
  margin: 0 0 16px 0;
}

.tip-footer {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
}

.heart-decoration {
  font-size: 14px;
  animation: heartbeat 1.5s ease-in-out infinite;
}

.heart-decoration:nth-child(3) {
  animation-delay: 0.3s;
}

@keyframes heartbeat {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.2); }
}

.footer-text {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 13px;
  color: #8B7B8E;
  font-weight: 500;
}
</style>
