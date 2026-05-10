<template>
  <div class="countdown-card" :class="{ overdue: isOverdue, ready: isReady }">
    <div class="countdown-header">
      <span class="subtitle">{{ isOverdue ? '宝宝已经' : '距离卸货还有' }}</span>
    </div>
    <div class="countdown-main">
      <div class="days" v-if="!isOverdue">
        <span class="number">{{ daysLeft }}</span>
        <span class="unit">天</span>
      </div>
      <div class="days overdue-text" v-else>
        <span class="number">{{ Math.abs(daysLeft) }}</span>
        <span class="unit">天</span>
      </div>
      <div class="time-info" v-if="!isOverdue">
        <span class="hours">{{ hoursLeft }}</span>
        <span class="time-unit">小时</span>
      </div>
    </div>
    <div class="week-info">
      <span class="week-badge">第 {{ currentWeek }} 周</span>
      <span class="days-badge" v-if="daysIntoWeek > 0">+ {{ daysIntoWeek }} 天</span>
    </div>
    <div class="breathing-halo"></div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  daysLeft: number;
  hoursLeft: number;
  currentWeek: number;
  daysIntoWeek: number;
  isOverdue: boolean;
  isReady: boolean;
}>();
</script>

<style scoped>
.countdown-card {
  position: relative;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.9) 0%, rgba(248, 240, 245, 0.9) 100%);
  border-radius: 24px;
  padding: 32px 24px;
  text-align: center;
  box-shadow: 0 8px 32px rgba(232, 180, 200, 0.3);
  overflow: hidden;
}

.countdown-card.overdue {
  background: linear-gradient(135deg, rgba(255, 235, 238, 0.95) 0%, rgba(255, 200, 210, 0.9) 100%);
}

.countdown-card.ready {
  background: linear-gradient(135deg, rgba(255, 245, 230, 0.95) 0%, rgba(255, 220, 180, 0.9) 100%);
}

.countdown-header {
  margin-bottom: 8px;
}

.subtitle {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 16px;
  color: #8B7B8E;
  font-weight: 400;
}

.countdown-main {
  display: flex;
  align-items: baseline;
  justify-content: center;
  gap: 12px;
  margin-bottom: 16px;
}

.days {
  display: flex;
  align-items: baseline;
  gap: 4px;
}

.number {
  font-family: 'Quicksand', sans-serif;
  font-size: 72px;
  font-weight: 700;
  color: #E8B4C8;
  line-height: 1;
  animation: breathe 3s ease-in-out infinite;
}

.countdown-card.overdue .number {
  color: #FF8A9B;
}

.countdown-card.ready .number {
  color: #FFB366;
}

@keyframes breathe {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.02); }
}

.unit {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 24px;
  color: #8B7B8E;
  font-weight: 500;
}

.time-info {
  display: flex;
  align-items: baseline;
  gap: 4px;
}

.hours {
  font-family: 'Quicksand', sans-serif;
  font-size: 28px;
  font-weight: 600;
  color: #B4D4E8;
}

.time-unit {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 14px;
  color: #8B7B8E;
}

.week-info {
  display: flex;
  justify-content: center;
  gap: 8px;
}

.week-badge, .days-badge {
  background: linear-gradient(135deg, #E8B4C8 0%, #B4D4E8 100%);
  color: white;
  padding: 6px 16px;
  border-radius: 20px;
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 14px;
  font-weight: 500;
}

.breathing-halo {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(232, 180, 200, 0.2) 0%, transparent 70%);
  animation: haloBreathe 4s ease-in-out infinite;
  pointer-events: none;
}

@keyframes haloBreathe {
  0%, 100% { transform: translate(-50%, -50%) scale(1); opacity: 0.5; }
  50% { transform: translate(-50%, -50%) scale(1.3); opacity: 0.2; }
}
</style>
