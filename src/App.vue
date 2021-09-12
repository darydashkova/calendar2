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
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
html, body{
  height: 100%;
  margin: 0;
}
</style>
