<script setup lang="ts">
interface User {
  name: string;
  email: string;
}

interface Category {
  title: string;
  description: string;
}

interface Post {
  id: number;
  user: User;
  category: Category;
  title: string;
  published_at: string;
  created_at: string;
  updated_at: string;
  content_raw: string;
  is_published: boolean;
}

const route = useRoute();
const postId = ref<number | null>(null);
const post = ref<Post | null>(null);

const getPost = async (id: number) => {
  try {
    const response = await $fetch<Post>(`http://127.0.0.1:8000/api/blog/posts/${id}`);
    post.value = response;
  } catch (error) {
    console.error('Error fetching post:', error);
  }
};

onMounted(() => {
  const id = parseInt(route.params.id as string, 10);
  if (!isNaN(id)) {
    postId.value = id;
    getPost(id);
  } else {
    console.error('Invalid post ID');
  }
});
</script>

<!-- pages/[id].vue -->
<template>
  <div class="container mx-auto p-4 text-gray-200 bg-gray-900">
    <div class="bg-gray-800 shadow-md rounded-lg p-6 mb-4">
      <h1 class="text-2xl font-bold mb-4">Post Details</h1>
      <div class="mb-4">
        <p class="text-gray-400"><strong>Post ID:</strong> {{ postId }}</p>
      </div>
      <ul class="mb-4">
        <li class="border-b border-gray-700 mb-2 pb-2"><strong>Title:</strong> {{ post?.title }}</li>
        <li class="border-b border-gray-700 mb-2 pb-2"><strong>Is published:</strong> {{ post?.is_published ? 'True' : 'False' }}</li>
        <li class="border-b border-gray-700 mb-2 pb-2"><strong>Published at:</strong> {{ post?.published_at }}</li>
        <li class="border-b border-gray-700 mb-2 pb-2"><strong>Created at:</strong> {{ post?.created_at }}</li>
        <li><strong>Updated at:</strong> {{ post?.updated_at }}</li>
      </ul>
      <div class="mt-4">
        <p class="text-gray-400"><strong>Content:</strong></p>
        <p>{{ post?.content_raw }}</p>
      </div>
    </div>

    <div class="flex flex-row gap-4">
      <div class="bg-gray-800 shadow-md rounded-lg p-6 w-1/2">
        <h2 class="text-xl font-bold mb-4">Category</h2>
        <ul>
          <li class="border-b border-gray-700 mb-2 pb-2"><strong>Title:</strong> {{ post?.category?.title }}</li>
          <li><strong>Description:</strong> {{ post?.category?.description || 'Empty' }}</li>
        </ul>
      </div>
      <div class="bg-gray-800 shadow-md rounded-lg p-6 w-1/2">
        <h2 class="text-xl font-bold mb-4">User</h2>
        <ul>
          <li class="border-b border-gray-700 mb-2 pb-2"><strong>Name:</strong> {{ post?.user?.name }}</li>
          <li><strong>Email:</strong> {{ post?.user?.email }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 800px;
}
.bg-gray-900 {
  background-color: #1a1a1a;
}
.bg-gray-800 {
  background-color: #2d2d2d;
}
.shadow-md {
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
.rounded-lg {
  border-radius: 0.5rem;
}
.text-gray-200 {
  color: #e5e7eb;
}
.text-gray-400 {
  color: #9ca3af;
}
.border-gray-700 {
  border-color: #374151;
}
</style>
