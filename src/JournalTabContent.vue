<template>
  <div>
    <h2 class="journal-title">Журнал заявок</h2>
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
        Немає заявок в журналі, що відповідають вашому запиту.
      </div>
      <div
        v-else-if="
          Object.keys(groupedRequests).length === 0 &&
          !searchQuery
        "
        class="no-results"
      >
        Журнал заявок порожній.
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

defineEmits(['update-status']);
</script>

<style scoped>
/* Add styles specific to JournalTabContent if needed */
</style>