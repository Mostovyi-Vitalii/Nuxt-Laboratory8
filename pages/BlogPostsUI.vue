<script setup lang="ts">
import axios from "axios";

const columns = [
  {key: 'id', label: '#'},
  {key: 'user_name', label: 'Автор'},
  {key: 'category_title', label: 'Категорія'},
  {key: 'title', label: 'Заголовок'},
  {key: 'published_at', label: 'Дата публікації'},
  {key: 'actions', label: 'Дії'}
]
const page = ref(1);
const page_count = 5;

interface User {
  name: string;
}

interface Category {
  id: number;
  title: string;
}

interface Post {
  id: number;
  user: User;
  category: Category;
  title: string;
  published_at: string;
}

const posts = ref<Post[]>([]);

const getPosts = async () => {
  try {
    const response = await $fetch<Post[]>(`http://127.0.0.1:8000/api/blog/posts`);
    posts.value = response;
  } catch (error) {
    console.error('Error fetching posts:', error);
  }
};
getPosts();

const rows = computed(() => {
  return posts.value.map((post) => ({
    id: post.id,
    user_name: post.user.name,
    category_title: post.category.title,
    title: post.title,
    published_at: post.published_at,
    category_id: post.category.id
  }));
});

const total = computed(() => rows.value.length);
const paginated_rows = computed(() => {
      return rows.value.slice((page.value - 1) * page_count, (page.value) * page_count);
    }
);

const items = (post:Post) => [
  [{
    label: 'Редагувати',
    icon: 'i-heroicons-pencil-square-20-solid',
    click: () => window.location.href = (`/posts/update/${post?.id}`)
  }, {
    label: 'Деталі',
    icon: 'i-heroicons-information-circle-20-solid',
    click: () => window.location.href = (`/posts/${post?.id}`)
  }], [{
    label: 'Видалити',
    icon: 'i-heroicons-trash-20-solid',
    click: () => {
      if (confirm(`Ви впевнені, що хочете видалити статтю '${post.title}'?`))
      {
        axios.delete(`http://127.0.0.1:8000/api/blog/posts/${post.id}`)
            .then(res => {
              console.log(res);
              alert('Пост видалено!');
              posts.value = posts.value.filter(p => p.id !== post.id);
            })
            .catch(error => {
              console.error(error);
              alert('Не вдалось видалити пост');
            });
      }
    }
  }]
]
</script>

<template>
  <UTable :columns="columns" :rows="paginated_rows" :total="rows.length" >
    <template #category_title-data="{ row }">
      <nuxt-link :to="`/categories/${row?.category_id}`" >
        {{row.category_title}}
      </nuxt-link>
    </template>
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