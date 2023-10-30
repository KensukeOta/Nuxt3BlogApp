<script setup lang="ts">
import type { Post } from "@/types/Post";

const props = defineProps<{
  post: Post,
}>();
</script>

<template>
  <article class="border p-2">
    <h2 class="font-bold">
      Hello
    </h2>

    <nav class="flex justify-between">
      by hoge
    </nav>
  </article>
</template>