<script setup lang="ts">
import type { authUser } from '~/types/authUser';

useHead({
  title: "ユーザー登録フォーム - Nuxt3BlogApp",
})

const runtimeConfig = useRuntimeConfig();

const { data, pending, error, refresh } = await useFetch<authUser>(`${runtimeConfig.public.apiUrl}/v1/user`, {
  headers: {
    "Accept": "application/json",
  },
  credentials: "include"
});
await refresh();

onMounted(async () => {
  if (data.value?.authUser) {
    await navigateTo("/");
  }
});
</script>

<template>
  <SignupForm />
</template>