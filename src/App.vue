<template>
  <div class="logobar"><img alt="Vue logo" src="./assets/logo.png"></div>
  <ResLib msg="Welcome to Your Vue.js App"/>
</template>

<script>
import ResLib from './components/SearchResults.vue'
import '@fortawesome/fontawesome-free/css/all.css';

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
}

body {
  margin: 0;
  padding: 0;
  background-color: #f5f5f5;
}

.logobar {
  padding: 20px 20px 15px 20px;
  background-color: #ffffff;
}

.logobar img {
  width: 175px;
  height: auto;
  display: inline;

}
</style>./components/SearchResults.vue
