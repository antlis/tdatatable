<script setup lang="ts">
import { ref, computed, watchEffect, watch } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import Pagination from '@components/Pagination.vue';
import Input from '@components/Input.vue';

interface Field {
  key: string
  label: string
  type: string
  sortable: boolean
}

interface Row {
  [key: string]: string
}

interface Props {
  fields: Field[]
  data: Row[]
  isPaginationShown?: Boolean
}

const props = withDefaults(defineProps<Props>(), {
  isPaginationShown: () => true,
});

const filter = ref('');
const currentPage = ref(1);
const perPage = ref(20);
const rows = ref(props.data.length);
const router = useRouter();
const route = useRoute();
const isPaginationShown = ref(true);
const sorted = ref<string[]>([]);
const data = ref(props.data);

const filteredRows = computed(() => {
  const filteredData = data.value.filter((row: any) => {
    for (const field of props.fields) {
      if (row[field.key].toString().toLowerCase().includes(filter.value.toLowerCase())) {
        return true;
      }
    }

    return false;
  });


  return filteredData
    .slice((currentPage.value - 1) * perPage.value, currentPage.value * perPage.value);
});

const handleSearch = (event: Event) => {
  filter.value = (event.target as HTMLInputElement).value;
}

const onPageSelect = (event: Event) => {
  onPageChange( +(event.target as HTMLInputElement).value );
}

const onPageChange = (pageNumber: number) => {
  currentPage.value = pageNumber;

}

const handleSort = (fieldKey: string) => {
  if (sorted.value.includes(fieldKey)) {
    sorted.value = [];
    data.value.sort((a, b) => b[fieldKey].localeCompare(a[fieldKey]));
  } else {
    sorted.value.push(fieldKey);
    data.value.sort((a, b) => a[fieldKey].localeCompare(b[fieldKey]));
  }
}

watchEffect(() => {
  if (filteredRows.value.length < perPage.value) {
    isPaginationShown.value = false;
  } else {
    isPaginationShown.value = true;
  }
});

watch(currentPage, (newVal, oldVal) => {
  console.log(route.path);
  if (newVal !== oldVal) {
    router.push({
      path: route.path,
      query: { page: currentPage.value }
    });
  }
});
</script>

<template>
  <div class="datatable">
    <div class="datatable__search-wrap">
      <Input @input="handleSearch" />
    </div>

    <table class="datatable__table">
      <thead>
        <tr>
          <th v-for="(field, key) in fields" :key="key">
            {{ field.label }}
            <button
              v-if="field.sortable"
              type="button"
              class="datatable__sort-btn"
              @click="handleSort(field.key)">
              {{ sorted.includes(field.key) ? '⬆️' : '⬇️' }}
            </button>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, index) in filteredRows" :key="index">
          <td v-for="(field, index) in fields" :key="index">
            <div v-if="field.type === 'img'">
              <img :src="row[field.key]" :alt="field.label" />
            </div>
            <div v-else-if="field.type === 'email'">
              <a :href="`mailto:row[field.key]`">{{ row[field.key] }}</a>
            </div>
            <div v-else-if="field.type === 'date'">
              {{
                new Date(row[field.key])
                  .toLocaleString('en-US', {
                    year: "numeric", month: "long", day: "numeric"
                  })
              }}
            </div>
            <div v-else-if="field.type === 'phone'">
              <a :href="`mailto:row[field.key]`">{{ row[field.key] }}</a>
            </div>
            <div v-else>
              {{ row[field.key] }}
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <div
      class="datatable__pagination-wrap">
      <Pagination
        v-if="isPaginationShown"
        :currentPage="currentPage"
        :rows="rows"
        :perPage="perPage"
        @goToPrevPage="onPageChange(currentPage -= 1)"
        @goToNextPage="onPageChange(currentPage += 1)"
        @goToSelectedPage="onPageSelect" />
    </div>
  </div>
</template>

<style scoped lang="scss">
.datatable {
  &__table  {
    width: 100%;
    max-width: 1200px;
    text-align: center;
  }

  &__search-wrap {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
  }

  &__pagination-wrap {
    margin-top: 20px;
  }

  &__sort-btn {
    border: transparent;
    background: transparent;
  }

  table, th, td {
    border: 1px solid;
  }

  th, td {
    padding: 5px;
  }
}
</style>
