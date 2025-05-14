<template>
  <div class="form-group">
    <label :for="id" class="form-label">{{ label }}</label>
    <component :is="type === 'textarea' ? 'textarea' : 'input'" :id="id" :value="modelValue" :type="type"
      :required="required" class="form-input" @input="$emit('update:modelValue', $event.target.value)">
    </component>
  </div>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      required: true,
    },
    id: {
      type: String,
      required: true,
    },
    modelValue: {
      type: [String, Number],
      default: '',
    },
    type: {
      type: String,
      default: 'text',
    },
    required: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style scoped>
.form-group {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-label {
  gap: 0.5rem;
  font-weight: 600;
  font-size: 0.875rem;
  color: var(--color-gray-600);
  text-align: center; /* Added text alignment */
}

.form-input {
  padding: 0.5rem;
  border: 1px solid var(--color-gray-300);
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.5);
  border-radius: 0.25rem;
  font-size: 0.875rem;
  width: 100%;
  /* width: initial; */
  margin: 0;
  box-sizing: border-box;
}

.form-input:focus {
  outline: none;
  border-color: var(--color-blue-500);
  box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.1);
}
</style>