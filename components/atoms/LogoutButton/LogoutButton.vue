<script setup lang="ts">
import { useForm } from "vee-validate";
import Cookies from "js-cookie";

const runtimeConfig = useRuntimeConfig();

const {
  handleSubmit,
  isSubmitting,
} = useForm();

const onSubmit = handleSubmit(async (values) => {
  try {
    await $fetch(`${runtimeConfig.public.apiUrl}/sanctum/csrf-cookie`, {
      credentials: "include"
    });
    await $fetch(`${runtimeConfig.public.apiUrl}/v1/users/logout`, {
      method: "POST",
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-XSRF-TOKEN": Cookies.get("XSRF-TOKEN") ?? "",
      },
      credentials: "include",
    });
    await navigateTo("/login");
  } catch (error: any) {
    console.log(error.data);
  }
});
</script>

<template>
  <form @submit="onSubmit" class="inline-block">
    <button :disabled="isSubmitting">ログアウト</button>
  </form>
</template>