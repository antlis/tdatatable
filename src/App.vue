<template>
  <Table
    :fields="fields"
    :data="data" />
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import api from './assets/api.json';

import Table from '@components/Table.vue';

const fields = ref([
  { key: 'avatar', label: 'Avatar', type: 'img', sortable: false },
  { key: 'fullName', label: 'Full Name', type: 'string', sortable: true },
  { key: 'gender', label: 'Gender', type: 'string', sortable: true },
  { key: 'country', label: 'Country', type: 'string', sortable: true },
  { key: 'dob', label: 'Date of birth', type: 'date', sortable: true },
  { key: 'email', label: 'Email', type: 'email', sortable: true },
  { key: 'phone', label: 'Phone', type: 'phone', sortable: false },
]);

const apiNormalized = api.results.map(({
  picture,
  name,
  gender,
  location,
  dob,
  email,
  phone
}) => {
  return {
    avatar: picture.medium,
    fullName: name.title + ' ' + name.first + ' ' + name.last,
    gender: gender,
    country: location.country,
    dob: dob.date,
    email: email,
    phone: phone,
  }
});

const data = ref(apiNormalized);
</script>
