<script setup lang="ts">
useHead({
  title: 'List of students',
  meta: [
    { name: 'description', content: 'My amazing site.' }
  ],
  bodyAttrs: {
    class: 'test'
  },
  script: [ { innerHTML: 'console.log(\'Hello world\')' } ]
})
const columns = [
  {key: 'title', label: 'Title',sortable: true},
  {key: 'description', label: 'Description',sortable: true},
  {key: 'price', label: 'Price',sortable: true},
  {key: 'rating', label: 'Rating',sortable: true},
  {key: 'brand', label: 'Brand',sortable: true},
  {key: 'category', label: 'Category',sortable: true},
  {key: 'thumbnail', label: 'Thumbnail',sortable: true}
]

const { data } = await useLazyFetch<any>('https://dummyjson.com/products')
let products = data.value.products
const q = ref('')
const page = ref(1)
const pageCount = 4
const total = ref(products.length)

const sort = ref({ column: 'title', direction: 'asc' as const })
const sortedRows = computed(() => {
  const sortedProducts = [...products]
  const { column, direction } = sort.value

  if (column && direction) {
    sortedProducts.sort((a, b) => {
      const aValue = a[column]
      const bValue = b[column]
      if (aValue < bValue) return direction === 'asc' ? -1 : 1
      if (aValue > bValue) return direction === 'asc' ? 1 : -1
      return 0
    })
  }
  console.log(sortedProducts)
  return sortedProducts
});

// Compute filtered and paginated rows based on the search query, sorted data, and pagination settings
const rows = computed(() => {
  let filteredProducts = [...sortedRows.value]

  if (q.value) {
    filteredProducts = filteredProducts.filter(product => {
      return Object.values(product).some(value => {
        return String(value).toLowerCase().includes(q.value.toLowerCase())
      })
    })
  }

  total.value = filteredProducts.length;
  const startIndex = (page.value - 1) * pageCount
  const endIndex = startIndex + pageCount

  return filteredProducts.slice(startIndex, endIndex)
})

const filteredRows = computed(() => {
  if (!q.value) {
    total.value = products.value.length;
    return products.value.slice((page.value - 1) * pageCount, (page.value) * pageCount);
  }
  page.value = 1;
  let result = products.value.filter((product: any) => {
    return Object.values(product).some(value =>
        String(value).toLowerCase().includes(q.value.toLowerCase())
    );
  });
  total.value = result.length;
  return result.slice((page.value - 1) * pageCount, (page.value) * pageCount);
});

const selected = ref([])

</script>

<template>
  <div>
    <div class="flex px-3 py-3.5 border-b border-gray-200 dark:border-gray-700">
      <UInput v-model="q" placeholder="Filter products..."/>
    </div>
    <UTable class="w-full" v-model:sort="sort" sort-mode="manual" v-model="selected" :ui="{ td: { base: 'mx-auto max-w-[0] truncate' } }" :columns="columns" :rows="rows">
      <template #thumbnail-data="{ row }">
        <img class="w-[100px] h-[100px]" :src="row.thumbnail" alt="Thumbnail" />
      </template>
      <template #rating-data="{ row }">
        <span :class="row.rating < 4.5 ? 'text-red-600' : 'text-green-600' ">{{ row.rating }}</span>
      </template>
    </UTable>
    <div class="flex justify-end px-3 py-3.5 border-t border-gray-200 dark:border-gray-700">
      <UPagination v-model="page" :page-count="pageCount" :total="total" />
    </div>
  </div>
</template>
