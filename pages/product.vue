<script setup lang="ts">
useHead({
  title: 'Products'
})
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
  label: 'Thumbnail'
}];

const { data } = await useFetch<any>('https://dummyjson.com/products');

const products = ref(data.value.products);

const q = ref('');
const filteredRows = computed(() => {
  if (!q.value) {
    return products.value;
  }

  return products.value.filter((product) => {
    return Object.values(product).some((value) => {
      return String(value).toLowerCase().includes(q.value.toLowerCase());
    });
  });
});

const page = ref(1);
const pageCount = 5;

const sort = ref({ column: '', direction: 'asc' as const })
const sortedRows = computed(() => {
  const sortedProducts = [...products.value]
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

  return sortedProducts
})

const rows = computed(() => {
  let filteredProducts = [...sortedRows.value]

  if (q.value) {
    filteredProducts = filteredProducts.filter(product => {
      return Object.values(product).some(value => {
        return String(value).toLowerCase().includes(q.value.toLowerCase())
      })
    })
  }

  const startIndex = (page.value - 1) * pageCount
  const endIndex = startIndex + pageCount

  return filteredProducts.slice(startIndex, endIndex)
})

</script>

<template>
  <div class="flex px-3 py-3.5 border-b border-gray-200 dark:border-gray-700">
    <UInput v-model="q" placeholder="Search products..." />
  </div>
  <UTable :rows="rows" :columns="productscolumns" v-model:sort="sort">
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