<template>
  <div class="replContainer">
    <Repl :editor="Monaco" :store="store" :showCompileOutput="false" :showTsConfig="false" :previewOptions="previewOptions" :showImportMap="false" />
  </div>
</template>

<script setup>
import { watchEffect, ref } from "vue";
import { Repl, useStore } from "@vue/repl";
import Monaco from "@vue/repl/monaco-editor";

const customStyle = () => {
  const link = '<link rel="stylesheet" href="https://unpkg.com/vitepress-vue3-components/dist/style.css" />';
  const style = "<style>body{ margin: 15px 10px;}</style>";
  return link + style;
};
const previewOptions = {
  headHTML: customStyle(),
};

// 安装默认依赖
const builtinImportMap = ref({
  imports: {
    // vue: "https://cdn.jsdelivr.net/npm/@vue/runtime-dom@3.5.13/dist/runtime-dom.esm-browser.js",
    // "vue/server-renderer": "https://cdn.jsdelivr.net/npm/@vue/server-renderer@3.5.13/dist/server-renderer.esm-browser.js",
    vue: "https://unpkg.com/@vue/runtime-dom@3.5.13/dist/runtime-dom.esm-browser.js",
    "vue/server-renderer": "https://unpkg.com/@vue/server-renderer@3.5.13/dist/server-renderer.esm-browser.js",
    "vitepress-vue3-components": "https://unpkg.com/vitepress-vue3-components/dist/index.es.js",
  },
});

const store = useStore(
  {
    builtinImportMap: builtinImportMap,
    showOutput: true,
    outputMode: "preview",
  },
  location.hash
);

watchEffect(() => history.replaceState({}, "", store.serialize()));
</script>

<style scoped>
.replContainer {
  position: absolute;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
}
</style>
