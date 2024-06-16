<script setup lang="ts">
import axios from "axios";

const columns = [
  {key: 'id', label: '#'},
  {key: 'title', label: 'Назва'},
  {key: 'slug', label: 'Силка'},
  {key: 'description', label: 'Опис'},
  {key: 'created_at', label: 'Дата створення'},
  {key: 'updated_at', label: 'Дата оновлення'},
  {key: 'deleted_at', label: 'Дата видалення'},
  {key: 'parent_title', label: 'Батьківська категорія'},
  {key: 'actions', label: 'Дії'}
]
const page = ref(1);
const page_count = 5;

interface Category {
  id: number;
  title: string;
  slug: string;
  description: string;
  created_at: string;
  updated_at: string;
  deleted_at: string;
  parent_id: number;
  parent_category: Category;
}

const categories = ref<Category[]>([]);

const getCategories = async () => {
  try {
    const response = await $fetch<Category[]>(`http://127.0.0.1:8000/api/blog/categories`);
    categories.value = response;
    console.log(categories.value);
  } catch (error) {
    console.error('Error fetching categories:', error);
  }
};
getCategories();

const rows = computed(() => {
  return categories.value.map((category) => ({
    id: category.id,
    title: category.title || 'none',
    description: category.description || 'none',
    slug: category.slug || 'none',
    parent_title: category.parent_category ? category.parent_category.title : 'none',
    created_at: category.created_at || 'none',
    updated_at: category.updated_at || 'none',
    deleted_at: category.deleted_at || 'none'
  }));
});

const total = computed(() => rows.value.length);
const paginated_rows = computed(() => {
      return rows.value.slice((page.value - 1) * page_count, (page.value) * page_count);
    }
);

const items = (category:Category) => [
  [{
    label: 'Редагувати',
    icon: 'i-heroicons-pencil-square-20-solid',
    click: () => window.location.href = (`/categories/update/${category?.id}`)
  }, {
    label: 'Деталі',
    icon: 'i-heroicons-information-circle-20-solid',
    click: () => window.location.href = (`/categories/${category?.id}`)
  }]
]
</script>

<template>
  <UTable :columns="columns" :rows="paginated_rows" :total="rows.length" >
    <template #actions-data="{ row }">
      <UDropdown :items="items(row)">
        <UButton color="gray" variant="ghost" icon="i-heroicons-ellipsis-horizontal-20-solid" />
      </UDropdown>
    </template>
  </UTable>
  <div>
    <UPagination v-model="page" :page-count="page_count" :total="total" />
  </div>
</template>