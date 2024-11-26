<!-- Sotr.vue -->
<template>
  <div v-if="employees.length" class="card p-4">
    <h3>Список сотрудников</h3>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>#</th>
          <th>Ф.И.О.</th>
          <th>Дата выдачи</th>
          <th>Отработанные дни</th>
          <th>Зарплата (без налогов)</th>
          <th>Зарплата (с налогами)</th>
          <th>Действия</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(employee, index) in employees" :key="employee.id">
          <td>{{ index + 1 }}</td>
          <td>{{ employee.name }}</td>
          <td>{{ employee.date }}</td>
          <td>{{ employee.daysWorked }}</td>
          <td>${{ employee.salary }}</td>
          <td>${{ (employee.salary * 0.85).toFixed(2) }}</td>
          <td>
            <button
              @click="removeEmployee(employee.id)"
              class="btn btn-danger btn-sm"
            >
              Удалить
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="text-end mt-4">
      <h5>Общая сумма без налогов: ${{ totalWithoutTaxes }}</h5>
      <h5>Общая сумма с налогами: ${{ totalWithTaxes.toFixed(2) }}</h5>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const employees = ref([]);

const removeEmployee = (id) => {
  employees.value = employees.value.filter((employee) => employee.id !== id);
};

const totalWithoutTaxes = computed(() => {
  return employees.value.reduce((sum, emp) => sum + Number(emp.salary), 0);
});

const totalWithTaxes = computed(() => {
  return totalWithoutTaxes.value * 0.85;
});

const emit = defineEmits();
defineProps({
  employees: {
    type: Array,
    required: true,
  }
});

</script>
