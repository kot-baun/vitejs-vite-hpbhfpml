<template>
<div
  class="request-card"
  :class="isMinimized ? 'card--minimized' : 'card--normal'"
>
  <div class="card-header">
    <div class="card-line">
      <div v-if="!isMinimized" class="department">Підрозділ: {{ request.department }}</div>
      <div class="request-text">Екіпаж: {{ request.executor }}</div>
      <div class="request-text">
        {{ request.timeBegin }} | {{ request.timeEnd }}
      </div>
    </div>
    <div class="details card-line">
      <div class="request-text">{{ request.vehicle }}</div>
      <div class="request-text">{{ request.weight }}</div>
    </div>
    <div class="card-line">
      {{ request.route }}
    </div>
  </div>

  <div
    class="my-status-section"
    :class="{ 'my-status-section--minimized': isMinimized }"
  >
    <button
      class="status-button prev-status"
      :disabled="getStatusIndex(request.status) === 0"
      @click="updateStatus(request, 'prev')"
    >
      &lt;&lt;
    </button>

    <div class="status-middle-area">
      <span class="status-text">{{ request.status }}</span>
      <div class="progress-bar">
        <div
          class="progress-bar-fill"
          :style="{ width: getStatusProgress(request.status) + '%' }"
        ></div>
      </div>
    </div>

    <button
      class="status-button next-status"
      :disabled="getStatusIndex(request.status) === statuses.length - 1"
      @click="updateStatus(request, 'next')"
    >
      &gt;&gt;
    </button>
  </div>

  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

// Define props using defineProps
const props = defineProps({
request: {
  type: Object,
  required: true,
},
statuses: {
  type: Array,
  required: true,
},
isMinimized: {
  type: Boolean,
  default: false,
},
});

// Define emitted events using defineEmits
const emit = defineEmits(['update-status']);

// Helper functions (can remain regular functions)
const getStatusIndex = (status) => {
return props.statuses.indexOf(status);
};

const getStatusProgress = (status) => {
const index = getStatusIndex(status);
return ((index + 1) / props.statuses.length) * 100;
};

// Method to update status and emit event
const updateStatus = (request, direction) => {
const currentIndex = getStatusIndex(request.status);
const newIndex =
  direction === 'next' ? currentIndex + 1 : currentIndex - 1;

if (newIndex >= 0 && newIndex < props.statuses.length) {
  // Emit an event to the parent to update the status
  emit('update-status', {
    request,
    newStatus: props.statuses[newIndex],
  });
}
};
</script>

<style scoped>
/* Add component-specific styles here if needed */
/* Styles for .request-card, .card-header, .department, .executor, .details, */
/* .status-section, .status-controls, .status-button, .status-text, */
/* .progress-bar, .progress-bar-fill should be added here or imported */
</style>
