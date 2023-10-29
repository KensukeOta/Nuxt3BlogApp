<script setup lang="ts">
import type { authUser } from '~/types/authUser';

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
  <header class="flex justify-between items-center border-b">
    <div class="leading-9">
      <button class="px-2 rounded-full hover:bg-gray-200">
        <i class="bi bi-list"></i>
      </button>

      <h1 class="inline-block">
        <NuxtLink to="/" class="inline-block">
          <NuxtImg src="/nuxt-logo.svg" alt="Nuxt Logo" width="24" height="24" class="inline-block" />
        </NuxtLink>
      </h1>
    </div>

    <nav class="leading-9">
      <button title="検索">
        <i class="bi bi-search"></i>
      </button>
      <div v-if="!data?.authUser" class="inline-block">
        <NuxtLink to="/signup" class="inline-block">新規登録</NuxtLink>
        <NuxtLink to="/login" class="inline-block">ログイン</NuxtLink>
      </div>
      <div v-else class="inline-block">
        <LogoutButton />
      </div>
    </nav>
  </header>
</template>