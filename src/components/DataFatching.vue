<script setup>
import { onMounted, reactive } from 'vue';

const countries  = reactive([]);

onMounted(async () => {
  try {
    const response = await fetch('https://restcountries.com/v3.1/all'); 
    const data = await response.json();
    countries.value = data;
  } catch (error) {
    console.error('Error fetching data:', error);
  }
});
</script>
<template>
    <div>
      <h2>All Country List</h2>
      <ul>
        <li v-for="country in countries" :key="country.id">{{ country.name.common }}</li>
      </ul>
    </div>
  </template>
  
 