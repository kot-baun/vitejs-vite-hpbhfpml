<template>
  <div class="form-title">Створити нову заявку</div>
  <form @submit.prevent="submitForm" class="request-form">
    <div class="form-section" style="grid-template-columns: 1fr 1fr">
      <div>
        <label for="department" class="form-label">Підрозділ:</label>
        <input id="department" v-model="formData.department" type="text" required class="form-input" />
      </div>
      <div>
        <label for="executor" class="form-label">Екіпаж:</label>
        <input id="executor" v-model="formData.executor" type="text" required class="form-input" />
      </div>
    </div>

    <div class="form-section" style="grid-template-columns: 3fr 1fr 1fr 1fr">
      <div>
        <label for="vehicleType" class="form-label">Засіб:</label>
        <input id="vehicleType" v-model="formData.vehicleType" type="text" required class="form-input" />
      </div>
      <div>
        <label for="frequencyControl" class="form-label">Частота данних:</label>
        <input id="frequencyControl" v-model="formData.frequencyControl" type="text" required class="form-input" />
      </div>
      <div>
        <label for="frequencyVideo" class="form-label">Частота відео:</label>
        <input id="frequencyVideo" v-model="formData.frequencyVideo" type="text" required class="form-input" />
      </div>
      <div>
        <label for="additionalFrequencies" class="form-label">Додаткові частоти:</label>
        <input id="additionalFrequencies" v-model="formData.additionalFrequencies" type="text" class="form-input" />
      </div>
    </div>

    <div class="form-section" style="grid-template-columns: 1fr 1fr 1fr 3fr">
      <div>
        <label for="altitude" class="form-label">Висота:</label>
        <input id="altitude" v-model="formData.altitude" type="text" required class="form-input" />
      </div>
      <div>
        <label for="timeBegin" class="form-label">Початок:</label>
        <input id="timeBegin" v-model="formData.timeBegin" type="time" required class="form-input" />
      </div>
      <div>
        <label for="timeEnd" class="form-label">Закінчення:</label>
        <input id="timeEnd" v-model="formData.timeEnd" type="time" required class="form-input" />
      </div>
      <div>
        <label for="takeoffPoint" class="form-label">Точка зльоту:</label>
        <input id="takeoffPoint" v-model="formData.takeoffPoint" type="text" required class="form-input" />
      </div>
    </div>

    <div class="form-section" style="grid-template-columns: 1fr">
      <div>
        <label for="route" class="form-label">Маршрут:</label>
        <textarea id="additionalInfo" v-model="formData.additionalInfo" class="form-input"
          @input="autoResize($event)"></textarea>
      </div>
    </div>

    <div class="form-section-contact" style="grid-template-columns: 1fr 1fr">

      <div>
        <label for="additionalInfo" class="form-label">Додатково:</label>
        <textarea id="additionalInfo" v-model="formData.additionalInfo" class="form-input"
          @input="autoResize($event)"></textarea>
      </div>

      <div>
        <label for="contacts" class="form-label">Контакти:</label>
        <textarea id="contacts" v-model="formData.contacts" class="form-input" required
          @input="autoResize($event)"></textarea>
        <div class="form-actions">
          <button type="submit" class="submit-button">Створити</button>
        </div>
      </div>
    </div>


  </form>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        department: '',
        executor: '',
        vehicleType: '',
        frequencyControl: '',
        frequencyVideo: '',
        additionalFrequencies: '',
        timeBegin: '',
        timeEnd: '',
        altitude: '',
        takeoffPoint: '',
        lbzPassage: '',
        route: '',
        contacts: '',
        additionalInfo: '',
      },
    };
  },
  emits: ['createRequest'],
  methods: {
    submitForm() {
      const newRequest = {
        ...this.formData,
        id: Date.now(),
        status: 'сформована',
      };
      this.$emit('createRequest', newRequest);
      this.resetForm();
    },
    resetForm() {
      Object.keys(this.formData).forEach((key) => (this.formData[key] = ''));
    },
    autoResize(event) {
      const textarea = event.target;
      textarea.style.height = 'auto'; // Reset height to calculate the new height
      textarea.style.height = `${textarea.scrollHeight}px`; // Set height to match content
    },
  },
};
</script>

<style scoped>
/* Material Design Styles */
body {
  background-color: #f5f5f5;
  font-family: 'Roboto', sans-serif;
}

.form-title {
  font-size: 1.5rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
  color: #333;
  text-align: center;
}

.request-form {
  padding: 0.5rem;
  background: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.form-section,
.form-section-contact {
  display: grid;
  gap: 1rem;
  justify-content: stretch;
  align-items: start;
  box-sizing: border-box;
}

.form-section {
  align-items: end;
}

.form-label {
  display: block;
  text-align: center;
  font-weight: 600;
  font-size: 0.875rem;
  color: #666;
}

.form-input {
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 0.5rem;
  background: #ffffff;
  font-size: 1rem;
  color: #333;
  outline: none;
  width: 100%;
  box-sizing: border-box;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

.form-input:focus {
  border-color: #6200ea;
  box-shadow: 0 0 0 2px rgba(98, 0, 234, 0.2);
}

textarea.form-input {
  resize: none;
  height: auto;
  overflow: hidden;
  /* Prevent scrollbars */
  transition: height 0.2s ease;
}

.form-input[type="time"] {
  text-align: center;
}

.form-actions {
  display: flex;
  justify-content: flex-end;
  margin-top: 1rem;
}

.submit-button {
  background: #6200ea;
  color: white;
  padding: 0.75rem 1.5rem;
  border-radius: 4px;
  border: none;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.submit-button:hover {
  background: #3700b3;
}
</style>