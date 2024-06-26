<template>
  <input
    class="input"
    :placeholder="placeholder"
    @input="$emit('input', $event)" />
</template>

<script lang="ts">
// import { ref, computed } from 'vue';

export default {
  props: {
    placeholder: { type: String, default: 'Search ...' },
  },
  setup(props) {

    return {
      ...props
    };
  },
};
</script>

<style scoped>
.input {
  font-size: 18px;
  height: 36px;
  width: 300px;
}
</style>
