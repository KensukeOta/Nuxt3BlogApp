<script setup lang="ts">
import type { authUser } from "@/types/authUser";
import type { Posts } from "@/types/Posts";

const runtimeConfig = useRuntimeConfig();

const { data, pending, error, refresh } = await useFetch<authUser>(`${runtimeConfig.public.apiUrl}/v1/user`, {
  headers: {
    "Accept": "application/json",
  },
  credentials: "include"
});
await refresh();

const { data: data2, pending: pending2, error: error2, refresh: refresh2 } = await useFetch<Posts>(`${runtimeConfig.public.apiUrl}/v1/posts`, {
  headers: {
    "Accept": "application/json",
  },
});
</script>

<template>
  <p>Welcome! {{ data?.authUser ? data.authUser.name : "stranger" }}</p>

  <nav class="text-center my-2">
    <PostLinkButton />
  </nav>

  <div v-if="data2?.posts.length as number > 0">
    <div v-for="post in data2?.posts" :key="post.id">
      <PostItem :post="post" :authUser="data?.authUser" />
    </div>
  </div>
  <div v-else>
    <p class="font-bold text-center p-2">記事が投稿されていません</p>
  </div>
</template>