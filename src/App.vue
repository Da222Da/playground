<template>
  <div class="replContainer">
    <Repl :editor="Monaco" :store="store" :showCompileOutput="true" :showTsConfig="false" :previewOptions="previewOptions" />
  </div>
</template>

<script setup>
import { watchEffect, ref } from "vue";
import { Repl, useStore, useVueImportMap } from "@vue/repl";
import Monaco from "@vue/repl/monaco-editor";

const customStyle = () => {
  const link = '<link rel="stylesheet" href="https://unpkg.com/zhangxianjue-vue3-components/dist/style.css" />';
  const style = "<style>body{ margin: 15px 10px;}</style>";
  return link + style;
};
const previewOptions = {
  headHTML: customStyle(),
};

// 安装默认依赖
const builtinImportMap = ref({
  imports: {
    vue: "https://cdn.jsdelivr.net/npm/@vue/runtime-dom@3.5.13/dist/runtime-dom.esm-browser.js",
    "vue/server-renderer": "https://cdn.jsdelivr.net/npm/@vue/server-renderer@3.5.13/dist/server-renderer.esm-browser.js",
    "zhangxianjue-vue3-components": "https://unpkg.com/zhangxianjue-vue3-components/dist/index.es.js",
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
