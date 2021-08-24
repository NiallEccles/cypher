<template>
  <Password msg="Hello Vue 3 + TypeScript + Vite" />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Password from "./components/Password.vue";

export default defineComponent({
  name: "App",
  components: {
    Password,
  },
});
</script>

<style>
@font-face {
  font-family: PlusJakartaSans-Medium;
  src: url("./assets/fonts/PlusJakartaSans-Medium.woff2");
}
body {
  color: #095C23;
  background: #fafbfa;
  margin: 0;
  padding: 0;
}
#app {
  font-family: PlusJakartaSans-Medium;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #095C23;
}
</style>
