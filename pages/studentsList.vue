<script setup lang="ts">
useHead({
  title: 'Students'
});
import { ref, computed } from 'vue';

const columns = [
  { key: 'id', label: 'ID', sortable: true },
  { key: 'name', label: 'Name', sortable: true },
  { key: 'surname', label: 'Surname', sortable: true },
  { key: 'specialty', label: 'Specialty', sortable: true },
  { key: 'group', label: 'Group', sortable: true },
  { key: 'email', label: 'Email', sortable: true }
];

const people = [
    { id: 7, name: 'Андрій', surname: 'Литвиненко', specialty: 'Маркетинг', group: '1', email: 'andriy.lytvynenko@example.com' },
    { id: 8, name: 'Олена', surname: 'Мельник', specialty: 'Психологія', group: '4', email: 'olena.melnyk@example.com' },
    { id: 9, name: 'Павло', surname: 'Шевченко', specialty: 'Менеджмент', group: '3', email: 'pavlo.shevchenko@example.com' },
    { id: 10, name: 'Анастасія', surname: 'Бондаренко', specialty: 'Література', group: '2', email: 'anastasiya.bondarenko@example.com' },
    { id: 11, name: 'Вікторія', surname: 'Козак', specialty: 'Інформатика', group: '1', email: 'viktoriya.kozak@example.com' },
    { id: 12, name: 'Дмитро', surname: 'Григоренко', specialty: 'Економіка', group: '4', email: 'dmytro.hryhorenko@example.com' },
    { id: 13, name: 'Юлія', surname: 'Павленко', specialty: 'Історія', group: '3', email: 'yuliya.pavlenko@example.com' },
    { id: 14, name: 'Олег', surname: 'Шевчук', specialty: 'Філософія', group: '2', email: 'oleg.shev@example.com' }
  ];

const q = ref('');
const page = ref(1);
const pageCount = 5;

const filteredRows = computed(() => {
  if (!q.value) {
    return people;
  }
  return people.filter((person) => {
    return Object.values(person).some((value) => {
      return String(value).toLowerCase().includes(q.value.toLowerCase());
    });
  });
});

const rows = computed(() => {
  if (!q.value) {
    return people.slice((page.value - 1) * pageCount, page.value * pageCount);
  } else {
    return filteredRows.value.slice((page.value - 1) * pageCount, page.value * pageCount);
  }
});
</script>

<template>
  <div>
    <div class="flex px-3 py-3.5 border-b border-gray-200 dark:border-gray-700">
      <UInput v-model="q" placeholder="Search people..." />
    </div>
    <UTable :rows="rows" :columns="columns" />
    <div class="flex justify-end px-3 py-3.5 border-t border-gray-200 dark:border-gray-700">
      <UPagination v-model="page" :page-count="pageCount" :total="filteredRows.length" />
    </div>
  </div>
</template>