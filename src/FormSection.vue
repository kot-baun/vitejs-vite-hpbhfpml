<template>
  <section class="form-section">
    <h4 v-if="title" class="section-title">{{ title }}</h4>
    <div class="section-content">
      <slot></slot>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: '',
    },
  },
};
</script>

<style scoped>
.form-section {
  margin-bottom: 0.3rem;
}

.section-title {
  font-size: 1rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: var(--color-gray-700);
}

.section-content {
  display: flex;
  flex-direction: row;
  gap: 1rem;
}
</style>