<template lang="pug">
.flex.flex-col.items-center.border.border-solid.border-warm-gray-400.rounded-xl.my-4
  .text-2xl.text-center.mt-4 Уже есть ключ?
  .text-lg.text-center.my-4 Загрузите
  transition(name="fade")
    .flex.flex-wrap.justify-center
      button.action.m-2.p-4(@click="show.text = !show.text")
        i.iconify(data-icon="la:key")
        .text Текст
      label.action.m-2.p-4(for="qr-input")
        i.iconify(data-icon="la:qrcode")
        .text QR-код
      label.action.m-2.p-4(for="json-input")
        i.iconify(data-icon="la:file-code")
        .text JSON-файл
      input#json-input.hidden(
        type="file",
        accept="application/json",
        @change="handleFile($event.target.files)"
      )
      util-load-qr.hidden(@loaded="handleText($event)")
  transition(name="fade")
    .flex.flex-col(v-if="show.text")
      textarea.my-4(
        @input="handleText($event.target.value)",
        placeholder="Вставьте сюда ваш ключ",
        :class="{ invalid: invalid }"
      )
</template>

<script setup>
import { participate } from "store@account";
import { reactive, ref } from "vue";

const show = reactive({
  options: true,
  text: false,
});

const invalid = ref(false);

function handleText(text) {
  if (text == "") invalid.value = false;
  if (text && text.includes("pub") && text.includes("priv")) {
    parseJSON(text);
  } else {
    invalid.value = true;
  }
}

function handleFile(files) {
  let file = files[0];
  if (file.size < 1000) {
    let reader = new FileReader();
    reader.readAsText(file);
    reader.onload = () => {
      parseJSON(reader.result);
    };
  }
}

function parseJSON(json) {
  if (json && json.includes("pub") && json.includes("priv")) {
    try {
      let obj = JSON.parse(json);
      participate(obj);
    } catch (e) {
      invalid.value = true;
    }
  }
}
</script>


