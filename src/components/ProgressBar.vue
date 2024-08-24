<template>
    <div class="progress-bar">
      <div class="progress-bar__labels">
        <span>0</span>
        <span>{{ total }}</span>
      </div>
      <div class="progress-bar__track">
        <div class="progress-bar__fill" :style="{ width: fillWidth + '%' }"></div>
      </div>
      <div
        class="progress-bar__current"
        :style="{ left: `calc(${fillWidth}% - 8px)` }"
      >
        {{ current }}
      </div>
    </div>
  </template>

  <script setup>
    import { computed } from 'vue';

    const props = defineProps({
      current: {
        type: Number,
        required: true
      },
      total: {
        type: Number,
        required: true
      }
    });

    const fillWidth = computed(() => {
      return Math.min(100, Math.max(0, (props.current / props.total) * 100));
    });
  </script>

  <style scoped>
    .progress-bar {
      position: relative;
    }

    .progress-bar__labels {
      display: flex;
      justify-content: space-between;
      margin-bottom: 3px;
      font-weight: 700;
      font-size: 16px;
    }

    .progress-bar__track {
      position: relative;
      height: 12px;
      margin-bottom: 3px;
      background-color: #f7f7f7;
      border-radius: 99px;
    }

    .progress-bar__fill {
      height: 100%;
      background-color: #30f;
      border-radius: 99px;
      transition: width 0.4s ease;
    }

    .progress-bar__current {
      position: absolute;
      font-weight: 700;
      font-size: 16px;
    }
  </style>
