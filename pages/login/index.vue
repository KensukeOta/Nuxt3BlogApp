<script setup lang="ts">
import type { authUser } from '~/types/authUser';

useHead({
  title: "ログインフォーム - Nuxt3BlogApp",
})

onMounted(async () => {
  const runtimeConfig = useRuntimeConfig();

  const { data, pending, error, refresh } = await useFetch<authUser>(`${runtimeConfig.public.apiUrl}/v1/user`, {
    headers: {
      "Accept": "application/json",
    },
    credentials: "include"
  });
  await refresh();

  if (data.value?.authUser) {
    await navigateTo("/");
  }
});
</script>

<template>
  <LoginForm />
</template>