<template lang="pug">
app-header
app-admin(v-if="account.is?.pub == currentRoom.host", :key="currentRoom.pub")
router-view(v-slot="{ Component }")
  transition(name="fade")
    component(:is="Component", :key="currentRoom.pub")
app-lab
</template>

<script setup >
import { currentRoom, enterRoom } from "store@room";
import { account } from "store@account";
import { watchEffect } from "vue";
import { useRoute } from "vue-router";
const route = useRoute();
watchEffect(() => {
  if (route.query.room) {
    enterRoom(route.query.room);
  }
});
</script>

<style lang="stylus">
button, button.action, label.action
  @apply: flex justify-center items-center rounded-full p-2 mx-2 cursor-pointer shadow-md bg-warm-gray-100

  svg
    font-size: 1.5em

  .text
    @apply: px-2

  $:hover
    @apply: bg-warm-gray-300

  $:active, $.active
    @apply: bg-warm-gray-200
</style>
