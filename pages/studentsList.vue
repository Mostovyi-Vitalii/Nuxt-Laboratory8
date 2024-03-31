<template>
  <div class="container mx-auto">
    <div class="flex justify-center mt-8">
      <UInput v-model="q" placeholder="Filter products..." />
    </div>
    <div class="mt-8">
      <div class="overflow-x-auto">
        <table class="w-full bg-white border border-gray-200 dark:border-gray-700">
          <thead>
          <tr class="bg-gray-100 dark:bg-gray-800">
            <th v-for="column in columns" :key="column.key" class="px-4 py-2 text-left text-gray-600 dark:text-gray-300">{{ column.label }}</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="row in rows" :key="row.id" class="border-t border-gray-200 dark:border-gray-700">
            <td v-for="column in columns" :key="column.key" class="px-4 py-2 text-black">
              <template v-if="column.key !== 'thumbnail' && column.key !== 'rating'">{{ row[column.key] }}</template>
              <template v-else-if="column.key === 'rating'">
                <span :class="{ 'text-green-600': row.rating >= 4.5, 'text-red-600': row.rating < 4.5 }">{{ row.rating }}</span>
              </template>
              <template v-else>
                <img :src="row.thumbnail" alt="Thumbnail" class="h-16 w-16 object-contain" />
              </template>
            </td>
          </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div class="flex justify-end mt-8">
      <UPagination v-model="page" :page-count="pageCount" :total="filteredRows.length" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

const columns = [
  {
    key: 'title',
    label: 'Title',
    sortable: true
  },
  {
    key: 'description',
    label: 'Description',
    sortable: true
  },
  {
    key: 'price',
    label: 'Price',
    sortable: true
  },
  {
    key: 'rating',
    label: 'Rating',
    sortable: true,
    color: 'ratingColor'
  },
  {
    key: 'brand',
    label: 'Brand',
    sortable: true
  },
  {
    key: 'category',
    label: 'Category',
    sortable: true
  },
  {
    key: 'thumbnail',
    label: 'Thumbnail'
  }
]

const { data } = await useLazyAsyncData<any>('products', () => $fetch('https://dummyjson.com/products'))

const products = data.value.products

const page = ref(1)
const pageCount = 5

const rows = computed(() => {
  return filteredRows.value.slice((page.value - 1) * pageCount, (page.value) * pageCount)
})

const q = ref('')

const filteredRows = computed(() => {
  if (!q.value) {
    return products
  }

  return products.filter((product: any) => {
    return Object.values(product).some((value) => {
      return String(value).toLowerCase().includes(q.value.toLowerCase())
    })
  })
})
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin-left: auto;
  margin-right: auto;
}
</style>
