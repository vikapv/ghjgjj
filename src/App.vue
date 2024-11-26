<script setup>
import { ref, computed } from "vue";

const employees = ref([]);

const name = ref('');
const date = ref('');
const daysWorked = ref(1);
const salary = ref(0);

const addEmployee = () => {
  if (name.value && date.value && daysWorked.value > 0 && salary.value > 0) {
    employees.value.push({
      id: Date.now(),
      name: name.value,
      date: date.value,
      daysWorked: daysWorked.value,
      salary: salary.value,
    });
    name.value = '';
    date.value = '';
    daysWorked.value = 1;
    salary.value = 0;
  }
};


const removeEmployee = (id) => {
  employees.value = employees.value.filter((employee) => employee.id !== id);
};

const totalWithoutTaxes = computed(() => {
  return employees.value.reduce((sum, emp) => sum + Number(emp.salary), 0);
});
const totalWithTaxes = computed(() => {
  return totalWithoutTaxes.value * 0.85;
});
</script>

<template>
  <div class="container">

    <div class="card p-4 mb-4">
      <h3>Добавить сотрудника</h3>
      <div class="mb-3">
        <label for="name" class="form-label">Ф.И.О.</label>
        <input
          type="text"
          id="name"
          v-model="name"
          class="form-control"
          :class="{'is-invalid': !name}"
        />
        <div class="invalid-feedback">Поле обязательно для заполнения</div>
      </div>
      <div class="mb-3">
        <label for="date" class="form-label">Дата выдачи зарплаты</label>
        <input
          type="date"
          id="date"
          v-model="date"
          class="form-control"
          :class="{'is-invalid': !date}"
        />
        <div class="invalid-feedback">Поле обязательно для заполнения</div>
      </div>
      <div class="mb-3">
        <label for="daysWorked" class="form-label">Количество отработанных дней</label>
        <input
          type="number"
          id="daysWorked"
          v-model="daysWorked"
          class="form-control"
          :class="{'is-invalid': !daysWorked || daysWorked <= 0}"
        />
        <div class="invalid-feedback">Поле обязательно для заполнения</div>
      </div>
      <div class="mb-3">
        <label for="salary" class="form-label">Размер заработной платы</label>
        <input
          type="number"
          id="salary"
          v-model="salary"
          class="form-control"
          :class="{'is-invalid': !salary || salary <= 0}"
        />
        <div class="invalid-feedback">Поле обязательно для заполнения</div>
      </div>
      <button @click="addEmployee" class="btn btn-dark">Добавить</button>
    </div>

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
  </div>
</template>