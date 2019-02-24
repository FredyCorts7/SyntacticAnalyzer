<template>
  <b-container id="app" fluid>
    <h1>Syntactic analyzer of Python with Vue js</h1> 
    <b-row class="justify-content-md-center" align-h="center">
      <img alt="Python logo" width="100" height="100" src="../public/imgs/logopython.png">
      <img alt="Vue logo" width="100" height="100" src="./assets/logo.png">
    </b-row>
    <HelloWorld/>
  </b-container>
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

@import '~bootstrap-vue/dist/bootstrap-vue.css';
@import '~bootstrap/dist/css/bootstrap.css';

  h1, p {
    font-weight: bolder !important;
    font-family: "varela round"
  }
  body {
    font-family: "varela round" !important;
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
