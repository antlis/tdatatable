<script setup lang="ts">
import { toRefs } from 'vue';

const props = defineProps<{
  currentPage: number
  perPage: number
  rows: number
}>()

const { currentPage, perPage, rows } = toRefs(props);
</script>

<template>
  <div class="pagination">
    <button
      v-if="currentPage > 1"
      class="pagination__btn"
      @click="$emit('goToPrevPage')">
      Prev
    </button>

    <select
      class="pagination__select"
      @change="($event) => $emit('goToSelectedPage', $event)">
      <option
        v-for="pageNum in Math.ceil(rows / perPage)"
        :selected="pageNum === currentPage">
        {{ pageNum }}
      </option>
    </select>

    <button
      v-if="currentPage < Math.ceil(rows / perPage)"
      class="pagination__btn"
      @click="$emit('goToNextPage')">
      Next
    </button>
  </div>
</template>

<style scoped lang="scss">
.pagination {
  display: flex;
  justify-content: center;
  gap: 20px;
  
  &__select {
    font-size: 18px;
    width: 100px;
    height: 32px;
  }

  &__btn {
    padding: 0 15px;
  }
}
</style>
