<template>
  <div class="p-5">
    <nuxt-link class="mt-20 bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded" to="/posts/create">Add post</nuxt-link>
  </div>
  <BlogPostsUI></BlogPostsUI>
</template>
<script setup lang="ts">
import BlogPostsUI from "~/pages/BlogPostsUI.vue";
</script>