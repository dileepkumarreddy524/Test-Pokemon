<template>
  <div id="app">
    <img src="./assets/logo.png" />
    <pokemon></pokemon>
  </div>
</template>

<script>
import Pokemon from "./components/Pokemons";
export default {
  name: "app",
  components: {
    Pokemon
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>