<script setup lang="ts">
interface Props {
  placeholder?: string
}

const props = withDefaults(defineProps<Props>(), {
  placeholder: 'Search ...',
});
</script>

<template>
  <input
    class="input"
    :placeholder="placeholder"
    @input="$emit('input', $event)" />
</template>

<style scoped>
.input {
  font-size: 18px;
  height: 36px;
  width: 300px;
}
</style>
