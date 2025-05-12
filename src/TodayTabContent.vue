<template>
  <div>
    <div class="today-header">
      <button class="create-button" @click="$emit('openRequestModal')">
        + створити
      </button>
      <label class="minimize-label">
        <input
          type="checkbox"
          :checked="isMinimized"
          @change="$emit('update:isMinimized', $event.target.checked)"
        />
        Minimize View
      </label>
      <input
        type="text"
        :value="searchQuery"
        @input="$emit('update:searchQuery', $event.target.value)"
        placeholder="Пошук за маршрутом..."
        class="search-input"
      />
    </div>
    <div class="department-groups">
      <details
        v-for="(requestsInDept, department) in groupedRequests"
        :key="department"
        class="department-details"
        open
      >
        <summary class="department-summary">
          {{ department }} ({{ requestsInDept.length }})
        </summary>
        <div
          class="requests-container"
          :class="isMinimized ? 'minimized' : 'normal-view'"
        >
          <request-card
            v-for="request in requestsInDept"
            :key="request.id"
            :request="request"
            :statuses="statuses"
            :is-minimized="isMinimized"
            @update-status="$emit('update-status', $event)"
          />
        </div>
      </details>
      <div
        v-if="
          Object.keys(groupedRequests).length === 0 &&
          searchQuery
        "
        class="no-results"
      >
        Немає заявок на сьогодні, що відповідають вашому запиту.
      </div>
      <div
        v-else-if="
          Object.keys(groupedRequests).length === 0 &&
          !searchQuery
        "
        class="no-results"
      >
        Наразі немає заявок на сьогодні.
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

defineProps({
  groupedRequests: Object,
  statuses: Array,
  searchQuery: String,
  isMinimized: Boolean,
});

defineEmits(['update-status', 'openRequestModal', 'update:isMinimized', 'update:searchQuery']);
</script>

<style scoped>
/* Add styles specific to TodayTabContent if needed */
</style>