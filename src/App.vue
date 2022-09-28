<template>
  <div id="app">
    <DemoAnimation />
  </div>
</template>

<script>
import DemoAnimation from "./components/DemoAnimation.vue";

export default {
  name: "App",
  components: {
    DemoAnimation,
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
