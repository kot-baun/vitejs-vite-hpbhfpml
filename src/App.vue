<template>
  <div class="main-layout">
    <Modal :visible="isOpenRequest" @closeRequestModal="closeRequestModal" @createRequest="handleCreateRequest" />
    <div class="main-content">
      <div class="content-wrapper">
        <ChatComponent />
      </div>
    </div>

    <div class="main-content">
      <div class="content-wrapper">
        <CreateRequestForm @createRequest="handleCreateRequest" />
      </div>
    </div>

    <div class="main-content">
      <div class="content-wrapper">
        <TabViewComponent :requests="requests" @update-status="handleStatusUpdate"
          @open-request-modal="openRequestModal" />
      </div>
    </div>

  </div>
</template>

<script setup>
import ChatComponent from './ChatComponent.vue';
import CreateRequestForm from './CreateRequestForm.vue';
// Import the requests data from the new file
import { ref } from 'vue';
import { requests as initialRequests } from './requestsData';
import TabViewComponent from './TabViewComponent.vue';
import Modal from './Modal.vue';

const requests = ref(initialRequests);

const handleCreateRequest = (newRequest) => {
  console.log('New request created:', newRequest);

  requests.value.push({
    id: requests.value.length + 1,
    ...newRequest,
    status: 'сформована',
  });
};

const handleStatusUpdate = ({ request, newStatus }) => {
  const index = requests.value.findIndex((r) => r.id === request.id);
  if (index !== -1) {
    requests.value[index].status = newStatus;
  }
};

const isOpenRequest = ref(false);

const openRequestModal = () => {
  isOpenRequest.value = true;
};

const closeRequestModal = () => {
  isOpenRequest.value = false;
};
</script>

<style>
/* Global styles if any */
</style>

<style scoped>
/* Add styles for the department grouping and summary */

/* request card */

/* Ensure status-text style is not overridden in the .request-card context if needed */
/* .status-text {
color: var(--color-gray-600);
font-size: 0.875rem;
} */
</style>
