<script setup lang="ts">
import type { Post } from "~/types/Post";
import { useForm } from "vee-validate";
import Cookies from "js-cookie";

const props = defineProps<{
  post: Post,
}>();

const runtimeConfig = useRuntimeConfig();

const {
  handleSubmit,
  defineInputBinds,
  isSubmitting,
} = useForm();

const onSubmit = handleSubmit(async (values) => {
  if (!confirm("この記事を削除しますか？")) {
    return;
  }
  
  try {
    await $fetch(`${runtimeConfig.public.apiUrl}/v1/posts/${props.post.id}`, {
      method: "DELETE",
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-XSRF-TOKEN": Cookies.get("XSRF-TOKEN") ?? "",
      },
      credentials: "include",
    });
    await navigateTo("/");
  } catch (error: any) {
    console.log(error.data);
  }
});
</script>

<template>
  <form @submit="onSubmit" class="inline-block">
    <button :disabled="isSubmitting" class="text-red-500">削除</button>
  </form>
</template>