<script setup lang="ts">
import "bootstrap-icons/font/bootstrap-icons.css";

useHead({
  title: "Nuxt3BlogApp",
  meta: [
    { name: "description", content: "BlogApp with Nuxt.js3" }
  ],
  htmlAttrs: {
    lang: "ja",
    class: "h-full"
  },
  bodyAttrs: {
    class: "h-full"
  },
})
</script>

<template>
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
</template>
