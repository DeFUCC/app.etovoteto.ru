<template lang="pug">
form.flex.flex-col(@submit.prevent)
  input.p-4.mt-4.mb-3.text-center.text-xl(
    v-model="search",
    ref="input",
    placeholder="Название комнаты"
  )
  .flex.flex-col.items-center(v-if="search.length > 3")
    button.p-4.rounded-full.bg-warm-gray-300(
      @click="create()",
      v-if="account.is?.pub"
    ) Создать
    my-auth(v-else)
</template>

<script setup>
import { createRoom } from "store@room";
import { search } from "model@room";
import { useRouter } from "vue-router";
import { onStartTyping } from "@vueuse/core";
import { account } from "store@account";
import { ref } from "vue";
const router = useRouter();

async function create() {
  const roomPub = await createRoom();
  search.value = "";
  router.push({ path: "/room/" + roomPub });
}

const input = ref(null);

onStartTyping(() => {
  if (!input.value.active) input.value.focus();
});
</script>

<style lang="stylus" scoped></style>