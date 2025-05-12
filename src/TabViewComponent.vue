<template>
  <div class="tabs-header">
    <div class="tabs-list">
      <button v-for="tab in tabs" :key="tab.id" :class="['tab-button', { 'tab-active': currentTab === tab.id }]"
        @click="currentTab = tab.id">
        {{ tab.name }}
      </button>
    </div>
  </div>
  <div class="tab-content">
    <div v-if="currentTab === 'active'">
      <div class="active-header">
        <button class="create-button" @click="openRequestModal">
          + створити
        </button>
        <label class="minimize-label">
          <input type="checkbox" v-model="isMinimized" />
          Minimize View
        </label>

        <input type="text" v-model="searchQuery" placeholder="Пошук за маршрутом..." class="search-input" />

      </div>

      <div class="department-groups">
        <details v-for="(requestsInDept, department) in groupedActiveRequests" :key="department"
          class="department-details" open>
          <summary class="department-summary">
            {{ department }} ({{ requestsInDept.length }})
          </summary>
          <div class="requests-container" :class="isMinimized ? 'minimized' : 'normal-view'">
            <request-card v-for="request in requestsInDept" :key="request.id" :request="request" :statuses="statuses"
              :is-minimized="isMinimized"
              @update-status="$emit('update-status', { request, newStatus: $event })"></request-card>
          </div>
        </details>
        <div v-if="
          Object.keys(groupedActiveRequests).length === 0 && searchQuery
        " class="no-results">
          Немає заявок, що відповідають вашому запиту.
        </div>
        <div v-else-if="
          Object.keys(groupedActiveRequests).length === 0 &&
          !searchQuery
        " class="no-results">
          Наразі немає активних заявок.
        </div>
      </div>
    </div>

    <div v-else-if="currentTab === 'today'">
      <div class="active-header">
        <button class="create-button" @click="openRequestModal">
          + створити
        </button>
        <label class="minimize-label">
          <input type="checkbox" v-model="isMinimized" />
          Minimize View
        </label>
        <input type="text" v-model="searchQuery" placeholder="Пошук за маршрутом..." class="search-input" />

      </div>
      <div class="department-groups">
        <details v-for="(requestsInDept, department) in groupedTodayRequests" :key="department"
          class="department-details" open>
          <summary class="department-summary">
            {{ department }} ({{ requestsInDept.length }})
          </summary>
          <div class="requests-container" :class="isMinimized ? 'minimized' : 'normal-view'">
            <request-card v-for="request in requestsInDept" :key="request.id" :request="request" :statuses="statuses"
              :is-minimized="isMinimized"
              @update-status="$emit('update-status', { request, newStatus: $event })"></request-card>
          </div>
        </details>
        <div v-if="
          Object.keys(groupedTodayRequests).length === 0 && searchQuery
        " class="no-results">
          Немає заявок на сьогодні, що відповідають вашому запиту.
        </div>
        <div v-else-if="
          Object.keys(groupedTodayRequests).length === 0 && !searchQuery
        " class="no-results">
          Наразі немає заявок на сьогодні.
        </div>
      </div>
    </div>

    <div v-else-if="currentTab === 'journal'">
      <h2 class="active-title">Журнал заявок</h2>
      <div class="department-groups">
        <details v-for="(requestsInDept, department) in groupedJournalRequests" :key="department"
          class="department-details" open>
          <summary class="department-summary">
            {{ department }} ({{ requestsInDept.length }})
          </summary>
          <div class="requests-container" :class="isMinimized ? 'minimized' : 'normal-view'">
            <request-card v-for="request in requestsInDept" :key="request.id" :request="request" :statuses="statuses"
              :is-minimized="isMinimized"
              @update-status="$emit('update-status', { request, newStatus: $event })"></request-card>
          </div>
        </details>
        <div v-if="
          Object.keys(groupedJournalRequests).length === 0 &&
          searchQuery
        " class="no-results">
          Немає заявок в журналі, що відповідають вашому запиту.
        </div>
        <div v-else-if="
          Object.keys(groupedJournalRequests).length === 0 &&
          !searchQuery
        " class="no-results">
          Журнал заявок порожній.
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import RequestCard from './RequestCard.vue';

const props = defineProps({
  requests: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(['update-status', 'open-request-modal']);

const currentTab = ref('active');
const isOpenRequest = ref(false);
const isMinimized = ref(false);
const searchQuery = ref('');

const tabs = [
  { id: 'active', name: 'Активні' },
  { id: 'today', name: 'Сьогодні' },
  { id: 'journal', name: 'Журнал' },
];

const statuses = [
  'сформована',
  'погрузка',
  'перевезення',
  'розгрузка',
  'завершено',
];

const openRequestModal = () => {
  emit('open-request-modal');
};

const activeRequests = computed(() => {
  return props.requests.filter((request) => request.status !== 'завершено');
});

const todayRequests = computed(() => {
  return props.requests.filter((request) => request.status !== 'завершено');
});

const journalRequests = computed(() => {
  return props.requests;
});

const filteredActiveRequests = computed(() => {
  if (!searchQuery.value) return activeRequests.value;
  return activeRequests.value.filter((request) =>
    request.route.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});

const filteredTodayRequests = computed(() => {
  if (!searchQuery.value) return todayRequests.value;
  return todayRequests.value.filter((request) =>
    request.route.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});

const filteredJournalRequests = computed(() => {
  if (!searchQuery.value) return journalRequests.value;
  return journalRequests.value.filter((request) =>
    request.route.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});

const groupedActiveRequests = computed(() => {
  return filteredActiveRequests.value.reduce((groups, request) => {
    const department = request.department || 'Без підрозділу';
    if (!groups[department]) {
      groups[department] = [];
    }
    groups[department].push(request);
    return groups;
  }, {});
});

const groupedTodayRequests = computed(() => {
  return filteredTodayRequests.value.reduce((groups, request) => {
    const department = request.department || 'Без підрозділу';
    if (!groups[department]) {
      groups[department] = [];
    }
    groups[department].push(request);
    return groups;
  }, {});
});

const groupedJournalRequests = computed(() => {
  return filteredJournalRequests.value.reduce((groups, request) => {
    const department = request.department || 'Без підрозділу';
    if (!groups[department]) {
      groups[department] = [];
    }
    groups[department].push(request);
    return groups;
  }, {});
});
</script>

<style scoped>
/* Add styles for the department grouping and summary */

/* request card */

/* Ensure status-text style is not overridden in the .request-card context if needed */
/* .status-text {
color: var(--color-gray-600);
font-size: 0.875rem;
} */
</style>
