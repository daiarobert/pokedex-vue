<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
  <logoBar />
  <test @activePokemon="getActivePokemon($event)" />
  <modal :activePokemon="activePokemon" />
</template>

<script>
import logoBar from "./components/logoBar.vue";

import modal from "./components/modal.vue";

import "bootstrap/dist/css/bootstrap.min.css";
import "bootstrap/dist/js/bootstrap.bundle";
import test from "./components/test.vue";

export default {
  components: { logoBar, test, modal },
  name: "App",
  data() {
    return {
      activePokemon: 1,
    };
  },
  methods: {
    getActivePokemon(data) {
      this.activePokemon = data;
      console.log("din app: " + this.activePokemon);
    },
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
  margin-top: 60px;
}
body {
  background-color: #f1f1f133;
}
</style>
