<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <ResLib msg="Welcome to Your Vue.js App"/>
</template>

<script>
import ResLib from './components/ResourceLibrary.vue'

export default {
  name: 'App',
  components: {
    ResLib
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #222;
  margin: 10px;
}
</style>
