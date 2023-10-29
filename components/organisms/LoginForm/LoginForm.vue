<script setup lang="ts">
import { useForm } from "vee-validate";
import { object, string } from "yup";
import { toTypedSchema } from '@vee-validate/yup';
import Cookies from "js-cookie";

const runtimeConfig = useRuntimeConfig();

const err = ref();

const schema = toTypedSchema(
  object({
    email: string().email("無効なメールアドレスの形式です").required("必須項目です"),
    password: string().required("必須項目です"),
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
    email: "",
    password: "",
  },
});

const onSubmit = handleSubmit(async (values) => {
  try {
    await $fetch(`${runtimeConfig.public.apiUrl}/sanctum/csrf-cookie`, {
      credentials: "include"
    });
    await $fetch(`${runtimeConfig.public.apiUrl}/v1/users/login`, {
      method: "POST",
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-XSRF-TOKEN": Cookies.get("XSRF-TOKEN") ?? "",
      },
      body: { email: values.email, password: values.password },
      credentials: "include",
    });
    await navigateTo("/");
  } catch (error: any) {
    err.value = error.data;
  }
});

const email = defineInputBinds("email", (state) => {
  return {
    validateOnInput: state.errors.length > 0
  };
});

const password = defineInputBinds("password", (state) => {
  return {
    validateOnInput: state.errors.length > 0
  };
});
</script>

<template>
  <form @submit="onSubmit" class="h-full flex items-center justify-center text-center">
    <fieldset class="border rounded-2xl grow">
      <legend class="px-2">ログイン</legend>
      <p class="text-red-500">{{ err?.message }}</p>
      <label>
        メールアドレス
        <input type="email" v-bind="email" name="email" id="email" class="border block mx-auto" />
      </label>
      <p class="text-red-500">{{ errors.email }}</p>

      <label>
        パスワード
        <input type="password" v-bind="password" name="password" id="password" class="border block mx-auto" />
      </label>
      <p class="text-red-500">{{ errors.password }}</p>

      <button :disabled="isSubmitting">ログイン</button>
    </fieldset>
  </form>
</template>