<template>
  <div class="p-5">
    <nuxt-link class="mt-20 bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded" to="/categories/create">Add category</nuxt-link>
  </div>
  <BlogCategoriesUI></BlogCategoriesUI>
</template>
<script setup lang="ts">
import BlogCategoriesUI from "~/pages/BlogCategoriesUI.vue";
</script>