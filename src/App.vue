<template>
  <div id="app">
    <workers-list />
  </div>
</template>

<script>
import WorkersList from "./components/WorkersList.vue";

export default {
  name: "app",
  components: {
    WorkersList
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
}
</style>
