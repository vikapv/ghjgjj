<template>
  <div class="container">
    <AddSotr @add-employee="addEmployee" />
    <Sotr :employees="employees" />
  </div>
</template>
<script setup>
import { ref } from "vue";
import AddSotr from './AddSotr.vue';
import Sotr from './Sotr.vue';

const employees = ref([]);

const addEmployee = (employee) => {
  employees.value.push(employee);
};
</script>