<script setup lang="ts">

</script>

<template>
  <form class="h-full flex items-center justify-center text-center">
    <fieldset class="border rounded-2xl grow">
      <legend class="px-2">登録</legend>
      <label>
        名前
        <input type="text" name="name" id="name" class="border block mx-auto" />
      </label>

      <label>
        メールアドレス
        <input type="email" name="email" id="email" class="border block mx-auto" />
      </label>

      <label>
        パスワード
        <input type="password" name="password" id="password" class="border block mx-auto" />
      </label>

      <label>
        パスワード確認
        <input type="password" name="password_confirmation" id="password_confirmation" class="border block mx-auto" />
      </label>

      <button>登録</button>
    </fieldset>
  </form>
</template>