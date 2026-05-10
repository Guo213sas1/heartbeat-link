<template>
  <Transition name="kick">
    <div v-if="visible" class="kick-feedback">
      <div class="kick-content">
        <span class="kick-icon">👶</span>
        <span class="kick-text">宝宝刚才踢了你一下!</span>
      </div>
    </div>
  </Transition>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';

const props = defineProps<{
  trigger: boolean;
}>();

const visible = ref(false);
let hideTimeout: ReturnType<typeof setTimeout> | null = null;

watch(() => props.trigger, (newVal) => {
  if (newVal) {
    visible.value = true;
    if (hideTimeout) clearTimeout(hideTimeout);
    hideTimeout = setTimeout(() => {
      visible.value = false;
    }, 3000);
  }
});
</script>

<style scoped>
.kick-feedback {
  position: fixed;
  top: 20%;
  left: 50%;
  transform: translateX(-50%);
  z-index: 200;
}

.kick-content {
  display: flex;
  align-items: center;
  gap: 12px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.95) 0%, rgba(248, 240, 245, 0.95) 100%);
  padding: 16px 24px;
  border-radius: 50px;
  box-shadow: 0 8px 32px rgba(232, 180, 200, 0.4);
  border: 2px solid rgba(232, 180, 200, 0.3);
}

.kick-icon {
  font-size: 28px;
  animation: kickBounce 0.5s ease-in-out infinite;
}

@keyframes kickBounce {
  0%, 100% { transform: translateY(0) rotate(-5deg); }
  50% { transform: translateY(-8px) rotate(5deg); }
}

.kick-text {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 16px;
  color: #5D4E60;
  font-weight: 500;
}

.kick-enter-active {
  animation: kickIn 0.5s ease-out;
}

.kick-leave-active {
  animation: kickOut 0.3s ease-in;
}

@keyframes kickIn {
  0% {
    opacity: 0;
    transform: translateX(-50%) translateY(-20px) scale(0.8);
  }
  100% {
    opacity: 1;
    transform: translateX(-50%) translateY(0) scale(1);
  }
}

@keyframes kickOut {
  0% {
    opacity: 1;
    transform: translateX(-50%) translateY(0) scale(1);
  }
  100% {
    opacity: 0;
    transform: translateX(-50%) translateY(-10px) scale(0.9);
  }
}
</style>
