<template lang="pug">
router-link.flex.flex.justify-start.p-2.my-4.text-xl.items-center.bg-warm-gray-50.shadow-lg(
  :to="'/author/' + author.pub"
)
  .flex.items-center.justify-start.flex-grow
    author-avatar.mr-2(:pub="author.pub", :size="84")
    .flex.flex-col.px-2
      .text-xl {{ author.name }}
      .text-base.font-normal {{ author.full }}
      .text-sm.font-normal.opacity-60 {{ author.city }}

  .flex.flex-col.justify-start
    .p-2
      i.iconify(data-icon="la:comment-dots")
      span.ml-1 {{ author.word }}
    .p-2
      i.iconify(data-icon="la:comment")
      span.ml-1 {{ author.def }}
</template>

<script setup>
import { defineProps, reactive, ref } from "vue";
import { gun } from "store@db";

const props = defineProps({
  author: Object,
});
</script>

<style lang="stylus" scoped>
a:hover
  text-decoration: none
  @apply: shadow-2xl
</style>