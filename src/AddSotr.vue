<template>
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
</template>

<script setup>
import { ref } from "vue";

const name = ref('');
const date = ref('');
const daysWorked = ref(1);
const salary = ref(0);
const emit = defineEmits();

const addEmployee = () => {
  if (name.value && date.value && daysWorked.value > 0 && salary.value > 0) {
    emit("add-employee", {
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
</script>
