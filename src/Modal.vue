<template>
  <teleport to="body">
    <div class="modal-overlay" v-if="visible" @click="closeModal">
      <div class="modal-content" @click.stop>
        <h3 class="modal-title">Створити нову заявку</h3>
        <form @submit.prevent="submitForm" class="request-form">
          <div class="form-grid">
            <div class="form-group">
              <label for="department">Підрозділ:</label>
              <input type="text" id="department" v-model="formData.department" required>
            </div>
            <div class="form-group">
              <label for="executor">Екіпаж:</label>
              <input type="text" id="executor" v-model="formData.executor" required>
            </div>
            <div class="form-group">
              <label for="vehicle">Транспортний засіб:</label>
              <input type="text" id="vehicle" v-model="formData.vehicle" required>
            </div>
            <div class="form-group">
              <label for="timeBegin">Час початку:</label>
              <input type="time" id="timeBegin" v-model="formData.timeBegin" required>
            </div>
            <div class="form-group">
              <label for="timeEnd">Час закінчення:</label>
              <input type="time" id="timeEnd" v-model="formData.timeEnd" required>
            </div>
            <div class="form-group">
              <label for="weight">Вага:</label>
              <input type="text" id="weight" v-model="formData.weight" required>
            </div>
          </div>
          <div class="form-group route-group">
            <label for="route">Маршрут:</label>
            <input type="text" id="route" v-model="formData.route" required>
          </div>
          <div class="form-actions">
            <button type="submit" class="submit-button">Створити</button>
            <button type="button" class="cancel-button" @click="closeModal">Скасувати</button>
          </div>
        </form>
        <button class="modal-close-button" @click="closeModal">×</button>
      </div>
    </div>
  </teleport>
</template>

<script>
export default {
  props: {
    visible: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      formData: {
        department: '',
        executor: '',
        vehicle: '',
        timeBegin: '',
        timeEnd: '',
        weight: '',
        route: '',
      }
    }
  },
  emits: ['closeRequestModal', 'createRequest'],
  methods: {
    closeModal() {
      this.$emit('closeRequestModal');
      this.resetForm();
    },
    handleKeydown(event) {
      if (this.visible && event.key === 'Escape') {
        this.closeModal();
      }
    },
    submitForm() {
      const newRequest = {
        ...this.formData,
        id: Date.now(),
        status: 'сформована'
      };
      this.$emit('createRequest', newRequest);
      this.closeModal();
    },
    resetForm() {
      this.formData = {
        department: '',
        executor: '',
        vehicle: '',
        timeBegin: '',
        timeEnd: '',
        weight: '',
        route: '',
      };
    }
  },
  mounted() {
    document.addEventListener('keydown', this.handleKeydown);
  },
  beforeUnmount() {
    document.removeEventListener('keydown', this.handleKeydown);
  },
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background-color: white;
  padding: 30px;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  max-width: 800px;
  width: 90%;
  position: relative;
}

.modal-title {
  margin: 0 0 20px 0;
  font-size: 1.5em;
  color: var(--color-gray-800);
}

.request-form {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.form-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 15px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.route-group {
  margin-top: 10px;
}

.form-group label {
  font-size: 0.9em;
  color: var(--color-gray-600);
}

.form-group input {
  padding: 8px;
  border: 1px solid var(--color-gray-200);
  border-radius: 4px;
  font-size: 1em;
}

.form-group input:focus {
  outline: none;
  border-color: var(--color-blue-500);
}

.form-actions {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

.submit-button {
  background-color: var(--color-blue-500);
  color: white;
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.submit-button:hover {
  opacity: 0.9;
}

.cancel-button {
  background-color: var(--color-gray-200);
  color: var(--color-gray-600);
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.cancel-button:hover {
  background-color: var(--color-gray-300);
}

.modal-close-button {
  position: absolute;
  top: 10px;
  right: 10px;
  background: none;
  border: none;
  font-size: 1.5em;
  cursor: pointer;
  color: var(--color-gray-600);
}

.modal-close-button:hover {
  color: var(--color-gray-800);
}
</style>