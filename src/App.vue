<template>
  <app-layout />
</template>

<script>
import appLayout from "./AppLayout.vue";

export default {
  name: "App",
  components: {
    appLayout,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
