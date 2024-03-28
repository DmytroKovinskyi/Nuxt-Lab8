<script setup lang="ts">
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
  { id: 1, name: 'Олексій', surname: 'Петренко', specialty: 'ДМ', group: '3', email: 'oleksiy.petrenko@example.com' },
  { id: 2, name: 'Валентина', surname: 'Ковальчук', specialty: 'ЕК', group: '1', email: 'valentyna.kovalchuk@example.com' },
  { id: 3, name: 'Тетяна', surname: 'Іванова', specialty: 'КН', group: '4', email: 'tetiana.ivanova@example.com' },
  { id: 4, name: 'Іван', surname: 'Сидоренко', specialty: 'Фінансист', group: '2', email: 'ivan.sydorenko@example.com' },
  { id: 5, name: 'Наталія', surname: 'Кравченко', specialty: 'Журналістика', group: '3', email: 'nataliya.kravchenko@example.com' },
  { id: 6, name: 'Михайло', surname: 'Бойко', specialty: 'КН', group: '2', email: 'mykhaylo.boiko@example.com' }
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
