<template>
  <Table
    :fields="fields"
    :data="data"
    isPaginationShown />
</template>

<script lang="ts">
import { ref } from 'vue';
import api from './assets/api.json';

import Table from './components/Table.vue';

export default {
  components: { Table },
  setup() {
    const fields = ref([
      { key: 'avatar', label: 'Avatar', type: 'img' },
      { key: 'fullName', label: 'Full Name', type: 'strin' },
      { key: 'gender', label: 'Gender', type: 'string' },
      { key: 'country', label: 'Country', type: 'strin' },
      { key: 'dob', label: 'Date of birth', type: 'date' },
      { key: 'email', label: 'Email', type: 'email' },
      { key: 'phone', label: 'Phone', type: 'phone' },
    ]);

    const apiNormalized = api.results.map((user) => {
      return {
        avatar: user.picture.medium,
        fullName: user.name.title + ' ' + user.name.first + ' ' + user.name.last,
        gender: user.gender,
        country: user.location.country,
        dob: user.dob.date,
        email: user.email,
        phone: user.phone,
      }
    });

    const data = ref(apiNormalized);

    // function handleFiltered(filteredRows: any[]) {
    //   rows.value = filteredRows.length;
    // }

    return {
      fields,
      data,
    };
  },
};
</script>
