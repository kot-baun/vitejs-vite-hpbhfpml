<template>
  <h3 class="form-title">Створити нову заявку</h3>
  <form @submit.prevent="submitForm" class="request-form">
    <FormSection>
      <FormGroup label="Підрозділ:" id="department" v-model="formData.department" required />
      <FormGroup label="Екіпаж:" id="executor" v-model="formData.executor" required />
      <FormGroup label="Засіб:" id="vehicleType" v-model="formData.vehicleType" required />
    </FormSection>

    <FormSection>
      <FormGroup label="Частота управління:" id="frequencyControl" v-model="formData.frequencyControl" required />
      <FormGroup label="Частота відео:" id="frequencyVideo" v-model="formData.frequencyVideo" required />
      <FormGroup label="Додаткові частоти:" id="additionalFrequencies" v-model="formData.additionalFrequencies" />
    </FormSection>

    <FormSection>
      <FormGroup label="Висота:" id="altitude" v-model="formData.altitude" required />
      <FormGroup label="Час початку:" id="timeBegin" v-model="formData.timeBegin" type="time" required />
      <FormGroup label="Час закінчення:" id="timeEnd" v-model="formData.timeEnd" type="time" required />
    </FormSection>

    <FormSection>
      <FormGroup label="Точка зльоту:" id="takeoffPoint" v-model="formData.takeoffPoint" required />
      <FormGroup label="Проходження ЛБЗ:" id="lbzPassage" v-model="formData.lbzPassage" />
      <FormGroup label="Маршрут:" id="route" v-model="formData.route" required />
    </FormSection>

    <FormSection>
      <FormGroup label="Контакти:" id="contacts" v-model="formData.contacts" required />
      <FormGroup label="Додатково:" id="additionalInfo" v-model="formData.additionalInfo" type="textarea" />
    </FormSection>

    <div class="form-actions">
      <button type="submit" class="submit-button">Створити</button>
    </div>
  </form>
</template>

<script>
import FormSection from './FormSection.vue';
import FormGroup from './FormGroup.vue';

export default {
  components: { FormSection, FormGroup },
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
  },
};
</script>

<style scoped>
.form-title {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 1rem;
  color: var(--color-gray-800);
}

.request-form {
  display: flex;
  flex-direction: column;
  /* gap: 1rem; */
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