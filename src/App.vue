<template>
  <div id="app">
    <h1>Syntactic analyzer of Python with Vue js</h1> 
    <b-row class="justify-content-md-center">
      <img alt="Python logo" width="100" height="100" src="../public/imgs/logopython.png">
      <img alt="Vue logo" width="100" height="100" src="./assets/logo.png">
    </b-row>
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
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    margin-top: 30px;
  }
</style>
