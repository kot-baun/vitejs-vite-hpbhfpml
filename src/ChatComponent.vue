<template>
<div class="content-wrapper">

  <div class="chat-container">
    <div class="contacts-column">
      <div class="contacts-header">
        <h3>Contacts</h3>
      </div>
      <ul class="contacts-list">
        <li
          v-for="contact in contacts"
          :key="contact.id"
          :class="[
            'contact-item',
            {
              'contact-active':
                selectedContact && selectedContact.id === contact.id,
            },
          ]"
          @click="selectContact(contact)"
        >
          <div class="contact-info">
            <span class="contact-name">{{ contact.name }}</span>
            <span class="contact-phone">{{ contact.phone }}</span>
          </div>
        </li>
      </ul>
    </div>

    <div class="chat-column">
      <div v-if="selectedContact" class="chat-window">
        <div class="chat-header">
          <h4>{{ selectedContact.name }}</h4>
        </div>
        <div class="messages-list">
          <div
            v-for="message in filteredMessages"
            :key="message.id"
            :class="[
              'message-bubble',
              message.sender === 'me' ? 'message-mine' : 'message-other',
            ]"
          >
            <div class="message-content">
              {{ message.text }}
            </div>
            <button
              v-if="message.sender === 'other'"
              class="process-button"
              @click="handleProcessClick(message)"
              title="Process Message"
            >
              ✨
            </button>
          </div>
        </div>
        <div class="chat-input-area">
          <input
            type="text"
            v-model="newMessageText"
            @keyup.enter="sendMessage"
            placeholder="Type a message..."
            class="message-input"
          />
          <button @click="sendMessage" class="send-button">Send</button>
        </div>
      </div>
      <div v-else class="no-contact-selected">
        Select a contact to start chatting.
      </div>
    </div>
  </div>
</div>
</template>

<script setup>
import { ref, computed } from 'vue';

// Define reactive state
const contacts = ref([
  { id: 1, name: 'Іван Петренко', phone: '+380 50 123 4567' },
  { id: 2, name: 'Марія Коваленко', phone: '+380 67 987 6543' },
  { id: 3, name: 'Олег Сидоренко', phone: '+380 93 111 2233' },
  { id: 4, name: 'Андрій Ковальчук', phone: '+380 95 555 4444' },
  { id: 5, name: 'Тетяна Мельник', phone: '+380 66 777 8899' },
]);

const messages = ref([
  { id: 1, contactId: 1, sender: 'other', text: 'Привіт! Як справи?' },
  { id: 2, contactId: 1, sender: 'me', text: 'Привіт! Все добре, дякую!' },
  {
    id: 3,
    contactId: 2,
    sender: 'other',
    text: 'Чи можемо обговорити заявку?',
  },
  { id: 4, contactId: 1, sender: 'other', text: 'Коли зручно поговорити?' },
  { id: 5, contactId: 3, sender: 'me', text: 'Так, готовий.' },
]);

// This would typically be managed by a parent component or state management
// For demonstration, we'll simulate adding to a list of processed requests here
const processedRequests = ref([]);

const selectedContact = ref(null);
const newMessageText = ref('');

// Computed property to filter messages for the selected contact
const filteredMessages = computed(() => {
  if (!selectedContact.value) {
    return [];
  }
  return messages.value.filter(
    (message) => message.contactId === selectedContact.value.id
  );
});

// Method to select a contact
const selectContact = (contact) => {
  selectedContact.value = contact;
};

// Method to send a new message
const sendMessage = () => {
  if (newMessageText.value.trim() === '' || !selectedContact.value) {
    return;
  }
  const newMessage = {
    id: messages.value.length + 1, // Simple ID generation
    contactId: selectedContact.value.id,
    sender: 'me', // Assuming messages sent from this UI are 'me'
    text: newMessageText.value.trim(),
  };
  messages.value.push(newMessage);
  newMessageText.value = ''; // Clear the input field
  // Optional: Scroll to the latest message (requires DOM manipulation, often done with nextTick)
};

// Method to handle the process button click and simulate request creation
const handleProcessClick = (message) => {
  console.log('Process button clicked for message:', message);

  // --- Simulate transforming message into a request ---
  // In a real app, you'd extract relevant info from the message.
  // This is a simplified example.
  const newRequest = {
    id: processedRequests.value.length + 100, // Example ID
    sourceMessageId: message.id, // Link to the original message
    contactId: message.contactId, // Link to the contact
    summary: message.text.substring(0, 50) + '...', // Use part of the message text
    status: 'сформована', // Set an initial status
    createdAt: new Date().toISOString(), // Timestamp
    // Add other request properties based on your data structure
    // e.g., department, executor, vehicle, route, etc.
    // You might need a more sophisticated way to parse the message text
    // or open a modal for the user to fill in request details.
  };

  processedRequests.value.push(newRequest);
  console.log('Simulated new request created:', newRequest);

  // --- Show Alert Window ---
  alert(`Message from ${selectedContact.value.name} processed into a request!`);

  // --- Potential next steps (depending on your app flow) ---
  // 1. Emit an event to the parent component:
  //    emit('create-request-from-message', newRequest);
  // 2. Call a store action to add the request to your global state.
  // 3. Open a modal pre-filled with message data for the user to confirm/edit.
};
</script>

<style scoped></style>
