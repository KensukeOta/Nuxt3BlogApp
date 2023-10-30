<script setup lang="ts">
import type { Post } from "@/types/Post";

const props = defineProps<{
  post: Post,
}>();
</script>

<template>
  <NuxtLink :to="`/${post.user.name}/posts/${post.id}/edit`" class="text-green-500">
    更新
  </NuxtLink>
</template>