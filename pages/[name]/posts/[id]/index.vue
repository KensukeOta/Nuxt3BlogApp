<script setup lang="ts">
import type { authUser } from '~/types/authUser';
import type { PostProps } from '~/types/PostProps';
import { MdPreview } from 'md-editor-v3';
import "github-markdown-css/github-markdown.css";

const id = "preview-only";

const runtimeConfig = useRuntimeConfig();

const route = useRoute();

const { data, pending, error, refresh } = await useFetch<authUser>(`${runtimeConfig.public.apiUrl}/v1/user`, {
  headers: {
    "Accept": "application/json",
  },
  credentials: "include"
});
await refresh();

const { data: data2, pending: pending2, error: error2, refresh: refresh2 } = await useFetch<PostProps>(`${runtimeConfig.public.apiUrl}/v1/posts/${route.params.id}`, {
  headers: {
    "Accept": "application/json",
  },
});

useHead({
  title: `${data2.value?.post.title} - Nuxt3BlogApp`
})
</script>

<template>
  <section>
    <h1 class="font-bold text-4xl">{{ data2?.post.title }}</h1>
    <p class="mt-1">by {{ data2?.post.user.name }}</p>

    <div class="mt-16">
      <div class="markdown-body">
        <MdPreview :editorId="id" :modelValue="data2?.post.body" />
      </div>
    </div>
  </section>
</template>