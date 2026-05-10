<template>
  <div class="fruit-comparison">
    <h3 class="comparison-title">宝宝现在的大小</h3>
    <div class="comparison-container">
      <div class="silhouette baby-silhouette">
        <div class="silhouette-inner baby-icon">
          <svg viewBox="0 0 100 100" class="baby-svg">
            <ellipse cx="50" cy="50" rx="35" ry="40" fill="rgba(232, 180, 200, 0.6)" />
            <ellipse cx="50" cy="35" rx="20" ry="18" fill="rgba(180, 212, 232, 0.5)" />
            <circle cx="43" cy="32" r="3" fill="rgba(93, 78, 96, 0.5)" />
            <circle cx="57" cy="32" r="3" fill="rgba(93, 78, 96, 0.5)" />
            <path d="M 45 40 Q 50 44 55 40" stroke="rgba(93, 78, 96, 0.5)" stroke-width="2" fill="none" />
            <ellipse cx="35" cy="60" rx="8" ry="5" fill="rgba(232, 180, 200, 0.4)" />
            <ellipse cx="65" cy="60" rx="8" ry="5" fill="rgba(232, 180, 200, 0.4)" />
          </svg>
        </div>
        <span class="silhouette-label">宝宝</span>
      </div>

      <div class="connector">
        <div class="dashed-line"></div>
        <span class="equals-sign">≈</span>
        <div class="dashed-line"></div>
      </div>

      <div class="silhouette fruit-silhouette">
        <div class="silhouette-inner fruit-icon">
          <span class="fruit-emoji">{{ fruitEmoji }}</span>
        </div>
        <span class="silhouette-label">{{ fruit }}</span>
      </div>
    </div>

    <div class="measurements">
      <div class="measurement">
        <span class="measurement-icon">📏</span>
        <span class="measurement-value">{{ length }}</span>
      </div>
      <div class="measurement">
        <span class="measurement-icon">⚖️</span>
        <span class="measurement-value">{{ weight }}</span>
      </div>
    </div>

    <p class="fruit-description">宝宝现在像一个 <strong>{{ fruit }}</strong> 那么大啦！</p>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  fruit: string;
  fruitEmoji: string;
  length: string;
  weight: string;
}>();
</script>

<style scoped>
.fruit-comparison {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.85) 0%, rgba(248, 240, 245, 0.85) 100%);
  border-radius: 24px;
  padding: 24px;
  box-shadow: 0 8px 32px rgba(180, 212, 232, 0.2);
}

.comparison-title {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 16px;
  color: #8B7B8E;
  text-align: center;
  margin: 0 0 20px 0;
  font-weight: 400;
}

.comparison-container {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 16px;
  margin-bottom: 20px;
}

.silhouette {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
}

.silhouette-inner {
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.6);
  border-radius: 50%;
  border: 2px dashed rgba(232, 180, 200, 0.4);
}

.baby-silhouette .silhouette-inner {
  animation: floatBaby 3s ease-in-out infinite;
}

@keyframes floatBaby {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-5px); }
}

.baby-svg {
  width: 70px;
  height: 70px;
}

.fruit-icon {
  font-size: 48px;
  animation: floatFruit 2.5s ease-in-out infinite;
}

@keyframes floatFruit {
  0%, 100% { transform: translateY(0) rotate(0deg); }
  50% { transform: translateY(-3px) rotate(3deg); }
}

.fruit-emoji {
  font-size: 48px;
}

.silhouette-label {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 14px;
  color: #8B7B8E;
  font-weight: 500;
}

.connector {
  display: flex;
  align-items: center;
  gap: 8px;
}

.dashed-line {
  width: 20px;
  height: 2px;
  background: repeating-linear-gradient(
    90deg,
    rgba(232, 180, 200, 0.6) 0px,
    rgba(232, 180, 200, 0.6) 4px,
    transparent 4px,
    transparent 8px
  );
}

.equals-sign {
  font-family: 'Quicksand', sans-serif;
  font-size: 24px;
  color: #E8B4C8;
  font-weight: 700;
}

.measurements {
  display: flex;
  justify-content: center;
  gap: 32px;
  margin-bottom: 16px;
}

.measurement {
  display: flex;
  align-items: center;
  gap: 6px;
}

.measurement-icon {
  font-size: 16px;
}

.measurement-value {
  font-family: 'Quicksand', sans-serif;
  font-size: 16px;
  color: #5D4E60;
  font-weight: 600;
}

.fruit-description {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 15px;
  color: #5D4E60;
  text-align: center;
  margin: 0;
  line-height: 1.6;
}

.fruit-description strong {
  color: #E8B4C8;
  font-weight: 700;
}
</style>
