<template>
  <kalendar></kalendar>
</template>

<script>

import kalendar from "./components/kalendar.vue";

export default {
  name: 'App',
  components: {
    kalendar,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
