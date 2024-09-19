<template>
  <div id="app">
    <!-- This is where your main application content goes -->
    <router-view /> <!-- If you're using Vue Router -->
  </div>
</template>

<script>
export default {
  name: "App",
};
</script>

<style>
/* Global styles for your app can go here */
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
