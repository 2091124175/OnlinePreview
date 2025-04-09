<script setup>
import { Sandbox, useStore } from "@vue/repl";
import { onMounted } from "vue";
import { strFromU8, strToU8, zlibSync } from 'fflate';

function utoa(data) {
  const buffer = strToU8(data);
  const zipped = zlibSync(buffer, { level: 9 });
  const binary = strFromU8(zipped, true);
  return btoa(binary);
}

let props = defineProps(["content"])
// 把内容 hash 化
const hash = utoa(
  JSON.stringify({
    "src/App.vue": props.content,
  })
)
const store = useStore({}, hash);
// onMounted(() => {
//   localStorage.setItem("content", "");
// });

</script>

<template>
  <Sandbox v-if="props.content" :store="store" style="height: 100vh" />
</template>

<style scoped></style>
