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
  { key: 'avatar', label: 'Avatar', type: 'img' },
  { key: 'fullName', label: 'Full Name', type: 'strin' },
  { key: 'gender', label: 'Gender', type: 'string' },
  { key: 'country', label: 'Country', type: 'strin' },
  { key: 'dob', label: 'Date of birth', type: 'date' },
  { key: 'email', label: 'Email', type: 'email' },
  { key: 'phone', label: 'Phone', type: 'phone' },
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
