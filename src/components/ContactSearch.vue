<template>
  <input
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      class="input" type="text" placeholder="Введите имя или фамилию">
  {{ modelValue }}
</template>

<script>
export default {
  name: "ContactSearch",
  props: ['modelValue'],
  emits: ["update:modelValue"]
}
</script>

<style scoped>
.input {
  border: 1px solid black;
  padding: 15px;
  border-radius: 10px;
  width: 100%;
  font-size: 16px;
  margin-bottom: 10px;
}
</style>