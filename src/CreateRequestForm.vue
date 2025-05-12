<template>
<div class="content-wrapper">

    <h3 class="form-title">Створити новий компонент</h3>
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
      </div>
    </form>
  </div>
</template>

<script>
export default {
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
  emits: ['createRequest'],
  methods: {
    submitForm() {
      const newRequest = {
        ...this.formData,
        id: Date.now(),
        status: 'сформована'
      };
      this.$emit('createRequest', newRequest);
      this.resetForm();
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
  }
};
</script>

<style scoped>
.form-container {
  background: white;
  border-radius: 0.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.5);
  padding: 1.5rem;
  max-width: 100%;
  margin: 1.5rem;
}

.form-title {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 1.5rem;
  color: var(--color-gray-800);
}

.request-form {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.form-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1rem;
}

@media (max-width: 500px) {
  .form-grid {
    grid-template-columns: 1fr;
  }
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-group label {
  font-size: 0.9rem;
  color: var(--color-gray-600);
}

.form-group input {
  padding: 0.5rem;
  border: 1px solid var(--color-gray-200);
  border-radius: 0.25rem;
  font-size: 1rem;
  width: 100%;
  box-sizing: border-box;
}

.form-group input:focus {
  outline: none;
  border-color: var(--color-blue-500);
  box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.1);
}

.route-group {
  margin-top: 0.5rem;
}

.form-actions {
  display: flex;
  justify-content: flex-end;
  margin-top: 1rem;
}

.submit-button {
  background-color: var(--color-blue-500);
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 0.25rem;
  border: none;
  cursor: pointer;
  font-size: 0.875rem;
  transition: background-color 0.2s;
}

.submit-button:hover {
  background-color: #2563eb;
}
</style>