<script setup lang="ts">
import type { authUser } from "@/types/authUser";

const runtimeConfig = useRuntimeConfig();

const { data, pending, error, refresh } = await useFetch<authUser>(`${runtimeConfig.public.apiUrl}/v1/user`, {
  headers: {
    "Accept": "application/json",
  },
  credentials: "include"
});
await refresh();
</script>

<template>
  <p>Welcome! {{ data?.authUser ? data.authUser.name : "stranger" }}</p>

  <nav class="text-center my-2">
    <PostLinkButton />
  </nav>
</template>