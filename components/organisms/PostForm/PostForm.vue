<script setup lang="ts">
import type { authUser } from "@/types/authUser"
import { MdPreview } from 'md-editor-v3';
import "github-markdown-css/github-markdown.css";
import { useForm } from "vee-validate";
import { number, object, string } from "yup";
import { toTypedSchema } from '@vee-validate/yup';
import Cookies from "js-cookie";

const id = 'preview-only';

const runtimeConfig = useRuntimeConfig();

const { data, pending, error, refresh } = await useFetch<authUser>(`${runtimeConfig.public.apiUrl}/v1/user`, {
  headers: {
    "Accept": "application/json",
  },
  credentials: "include"
});
await refresh();

const err = ref();

const schema = toTypedSchema(
  object({
    title: string().required("必須項目です"),
    body: string().max(10000, "10000文字以内で入力してください").required("必須項目です"),
    user_id: number().required(),
  }),
);

const {
  values,
  errors,
  handleSubmit,
  defineInputBinds,
  isSubmitting,
} = useForm({
  validationSchema: schema,
  initialValues: {
    title: "",
    body: "",
    user_id: data.value?.authUser.id,
  },
});

const onSubmit = handleSubmit(async (values) => {
  try {
    await $fetch(`${runtimeConfig.public.apiUrl}/v1/posts`, {
      method: "POST",
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-XSRF-TOKEN": Cookies.get("XSRF-TOKEN") ?? "",
      },
      body: { title: values.title, body: values.body, user_id: values.user_id },
      credentials: "include",
    });
    await navigateTo("/");
  } catch (error: any) {
    err.value = error.data;
  }
});

const title = defineInputBinds("title", (state) => {
  return {
    validateOnInput: state.errors.length > 0
  };
});

const body = defineInputBinds("body", (state) => {
  return {
    validateOnInput: state.errors.length > 0
  };
});

const user_id = defineInputBinds("user_id");
</script>

<template>
  <form @submit="onSubmit" class="h-full">
    <p class="text-red-500">{{ err?.message }}</p>
    <p class="text-red-500">{{ errors.title }}</p>
    <input type="text" v-bind="title" name="title" id="title" placeholder="タイトル" class="block border w-full p-2" />

    <p class="text-red-500">{{ errors.body }}</p>
    <div class="markdown-body flex h-[calc(100%-8.125rem)]">
      <textarea v-bind="body" name="body" id="body" placeholder="マークダウン記法で記述することができます。" class="flex-1 bg-gray-200" />

      <div class="flex-1">
        <MdPreview :editorId="id" :modelValue="values.body" />
      </div>
    </div>

    <input type="hidden" v-bind="user_id" name="user_id" id="user_id" />

    <button :disabled="isSubmitting" class="block bg-blue-400 rounded-3xl w-48 mx-auto text-white py-2 hover:bg-blue-300">投稿する</button>
  </form>
</template>