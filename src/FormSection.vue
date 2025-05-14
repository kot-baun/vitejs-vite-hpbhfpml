<template>
  <div class="form-section" :style="{ gridTemplateColumns: columns }">
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    columns: {
      type: String,
      default: '1fr', // Значення за замовчуванням: перша колонка для підпису, друга для поля введення
    },
  },
};
</script>

<style scoped>
.form-section {
  display: grid;
  grid-auto-flow: row; /* Елементи розташовуються в рядок */
  gap: 1rem; /* Відступ між елементами */
  align-items: center; /* Вирівнювання елементів по вертикалі */
  width: 100%;
}
</style>