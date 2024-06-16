<script setup lang="ts">
interface Category {
  id: number;
  title: string;
  slug: string;
  description: string;
  created_at: string;
  updated_at: string;
  deleted_at: string;
  parent_id: number;
}

const route = useRoute();
const categoryId = ref<number | null>(null);
const category = ref<Category | null>(null);

const getCategory = async (id: number) => {
  try {
    const response = await $fetch<Category>(`http://127.0.0.1:8000/api/blog/categories/${id}`);
    category.value = response;
  } catch (error) {
    console.error('Error fetching category:', error);
  }
};

onMounted(() => {
  const id = parseInt(route.params.id as string, 10);
  if (!isNaN(id)) {
    categoryId.value = id;
    getCategory(id);
  } else {
    console.error('Invalid Category ID');
  }
});
</script>

<template>
  <div class="container mx-auto mt-10 p-4 max-w-lg">
    <div class="mb-6 text-center">
      <nuxt-link class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded" to="/categories">
        Back
      </nuxt-link>
    </div>
    <UCard>
      <template #header>
        <h2 class="text-xl font-semibold mb-4">Category Details</h2>
      </template>
      <div class="mb-4">
        <p><strong>ID:</strong> {{ categoryId }}</p>
      </div>
      <ul class="space-y-3">
        <li><strong>Title:</strong> {{category?.title}}</li>
        <li><strong>Slug:</strong> {{category?.slug}}</li>
        <li><strong>Description:</strong> {{category?.description ? category.description : 'none'}}</li>
        <li><strong>Created at:</strong> {{category?.created_at ? category.created_at : 'none'}}</li>
        <li><strong>Updated at:</strong> {{category?.updated_at ? category.updated_at : 'none'}}</li>
        <li><strong>Deleted at:</strong> {{category?.deleted_at ? category.deleted_at : 'none'}}</li>
      </ul>
      <template #footer>
        <div class="mt-4">
          <p><strong>Parent id:</strong> {{category?.parent_id ? category.parent_id : 'none'}}</p>
        </div>
      </template>
    </UCard>
  </div>
</template>
