<script setup lang="ts">
import { useForm } from "vee-validate";
import { object, string, ref as Ref } from "yup";
import { toTypedSchema } from '@vee-validate/yup';
import Cookies from "js-cookie";

const runtimeConfig = useRuntimeConfig();

const err = ref();

const schema = toTypedSchema(
  object({
    name: string().max(10, "10文字以内で入力してください").required("必須項目です"),
    email: string().email("無効なメールアドレスの形式です").required("必須項目です"),
    password: string().min(6, "6文字以上、30文字以内で入力してください").max(30, "6文字以上、30文字以内で入力してください").required("必須項目です"),
    password_confirmation: string().oneOf([Ref("password")], "パスワードが一致しません。").required("必須項目です"),
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
    name: "",
    email: "",
    password: "",
    password_confirmation: "",
  },
});

const onSubmit = handleSubmit(async (values) => {
  try {
    await $fetch(`${runtimeConfig.public.apiUrl}/sanctum/csrf-cookie`, {
      credentials: "include"
    });
    await $fetch(`${runtimeConfig.public.apiUrl}/v1/users`, {
      method: "POST",
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-XSRF-TOKEN": Cookies.get("XSRF-TOKEN") ?? "",
      },
      body: { name: values.name, email: values.email, password: values.password, password_confirmation: values.password_confirmation },
      credentials: "include",
    });
    await navigateTo("/");
  } catch (error: any) {
    err.value = error.data;
  }
});

const name = defineInputBinds("name", (state) => {
  return {
    validateOnInput: state.errors.length > 0
  };
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

const password_confirmation = defineInputBinds("password_confirmation", (state) => {
  return {
    validateOnInput: state.errors.length > 0
  };
});
</script>

<template>
  <form @submit="onSubmit" class="h-full flex items-center justify-center text-center">
    <fieldset class="border rounded-2xl grow">
      <legend class="px-2">登録</legend>
      <p class="text-red-500">{{ err?.message }}</p>
      <label>
        名前
        <input type="text" v-bind="name" name="name" id="name" class="border block mx-auto" />
      </label>
      <p class="text-red-500">{{ errors.name }}</p>

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

      <label>
        パスワード確認
        <input type="password" v-bind="password_confirmation" name="password_confirmation" id="password_confirmation" class="border block mx-auto" />
      </label>
      <p class="text-red-500">{{ errors.password_confirmation }}</p>

      <button :disabled="isSubmitting">登録</button>
    </fieldset>
  </form>
</template>