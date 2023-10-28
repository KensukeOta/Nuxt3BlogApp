<script setup lang="ts">

</script>

<template>
  <form class="h-full flex items-center justify-center text-center">
    <fieldset class="border rounded-2xl grow">
      <legend class="px-2">ログイン</legend>

      <label>
        メールアドレス
        <input type="email" name="email" id="email" class="border block mx-auto" />
      </label>

      <label>
        パスワード
        <input type="password" name="password" id="password" class="border block mx-auto" />
      </label>

      <button>ログイン</button>
    </fieldset>
  </form>
</template>