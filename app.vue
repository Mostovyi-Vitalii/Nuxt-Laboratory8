<style>
/* Глобальні стилі */
body, html {
  height: 100%;
  margin: 0;
  font-family: Arial, sans-serif;
}

/* Стилі для контейнера з кнопками */
.button-container {
  display: flex;
  justify-content: space-between;
  padding: 20px;
  background-color: #f0f0f0; /* фон контейнера */
}

/* Стилі для кнопок */
.button7 {
  font-weight: 700;
  color: white;
  text-decoration: none;
  padding: 1em 2em;
  border-radius: 3px;
  background: rgb(64, 199, 129);
  box-shadow: 0 -3px rgb(53, 167, 110) inset;
  transition: 0.2s;
}

.button7:hover {
  background: rgb(53, 167, 110);
}

.button7:active {
  background: rgb(33, 147, 90);
  box-shadow: 0 3px rgb(33, 147, 90) inset;
}
</style>

<template>
  <div>
    <div class="button-container">
      <a class="button7" href="/product">Product</a>
      <a class="button7" href="/studentsList">Students</a>
      <a class="button7" href="/BlogPosts">BlogPosts</a>
      <a class="button7" href="/BlogPostsUI">BlogPostsUI</a>
      <a class="button7" href="/onlyoneshow/1">1</a>
    </div>
    <hr>

    <NuxtPage/>
  </div>
</template>

<script setup lang="ts">
</script>
