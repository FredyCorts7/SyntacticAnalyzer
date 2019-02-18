<template>
  <div id="app">
    <img alt="Python logo" width="100" height="100" src="../public/imgs/logopython.png">
    <img alt="Vue logo" width="100" height="100" src="./assets/logo.png">
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script>

<style>
  body {
    margin: 0;
    padding: 0;
  }
  #app {
    font-family: sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 30px;
  }
</style>
