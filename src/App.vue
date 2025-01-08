<template>
  <div class="replContainer">
    <Repl :editor="Monaco" :store="store" :showCompileOutput="false" :showTsConfig="false" />
  </div>
</template>

<script setup>
import { watchEffect } from "vue";
import { Repl, useStore, useVueImportMap } from "@vue/repl";
import Monaco from "@vue/repl/monaco-editor";

const { importMap: builtinImportMap } = useVueImportMap({
  // serverRenderer: 'cdn link to server-renderer.esm-browser.js',
});

const store = useStore(
  {
    builtinImportMap,
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
