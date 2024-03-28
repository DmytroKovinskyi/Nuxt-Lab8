<script setup lang="ts">
const productscolumns = [{
  key: 'id',
  label: 'ID',
  sortable: true
}, {
  key: 'title',
  label: 'Title',
  sortable: true
}, {
  key: 'description',
  label: 'Description',
  sortable: true
}, {
  key: 'price',
  label: 'Price',
  sortable: true
}, {
  key: 'rating',
  label: 'Rating',
  sortable: true
},{
  key: 'brand',
  label: 'Brand',
  sortable: true
},{
  key: 'category',
  label: 'Category',
  sortable: true
}, {
  key: 'thumbnail',
  label: 'Thumbnail',
  sortable: true
}];

const q = ref('')
const page = ref(1)
const pageCount = 5

const { data: count } = await useFetch<any>('https://dummyjson.com/products')
console.log(count.value);
const { products } = count.value;

const filteredRows = computed(() => {
  if (!q.value) {
    return products
  }
  return products.filter((products) => {
    return Object.values(products).some((value) => {
      return String(value).toLowerCase().includes(q.value.toLowerCase())
    })
  })
})

const rows = computed(() => {
  if (!q.value) {
    return products.slice((page.value - 1) * pageCount, (page.value) * pageCount)
  }

  return products.filter((products) => {
    return Object.values(products).some((value) => {
      return String(value).toLowerCase().includes(q.value.toLowerCase())
    })
  })
})
</script>

<template>
  <div class="flex px-3 py-3.5 border-b border-gray-200 dark:border-gray-700">
    <UInput v-model="q" placeholder="Search products..." />
  </div>
  <UTable :rows="rows" :columns="productscolumns">
    <template #thumbnail-data="{ row }">
      <img class="w-[100px] h-[100px]" :src="row.thumbnail" alt="Thumbnail" />
    </template>
    <template #rating-data="{ row }">
      <span :class="{ 'text-green-500': row.rating > 4.5, 'text-red-500': row.rating <= 4.5 }">{{ row.rating }}</span>
    </template>
  </UTable>
  <div class="flex justify-end px-3 py-3.5 border-t border-gray-200 dark:border-gray-700">
    <UPagination
        v-model="page"
        :page-count="pageCount"
        :total="filteredRows.length"
    />
  </div>
</template>