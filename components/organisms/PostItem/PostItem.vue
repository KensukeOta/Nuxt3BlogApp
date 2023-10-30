<script setup lang="ts">
import type { Post } from "@/types/Post";
import type { User } from "~/types/User";

const props = defineProps<{
  post: Post,
  authUser?: User,
}>();
</script>

<template>
  <article class="border p-2">
    <h2 class="font-bold">
      <NuxtLink :to="`/${post.user.name}/posts/${post.id}`" class="inline-block w-full hover:underline">
        {{ post.title }}
      </NuxtLink>
    </h2>

    <nav class="flex justify-between">
      by {{ post.user.name }}
      <PostEditLinkButton :post="post" v-if="authUser?.id === post.user_id" />
      <PostDeleteButton :post="post" v-if="authUser?.id === post.user_id" />
    </nav>
  </article>
</template>