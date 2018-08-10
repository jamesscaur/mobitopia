<template>
  <div id="app">
    <img class="cryptopia-logo" src="./assets/logo2.png">
    <Home/>
  </div>
</template>

<script>
import Home from './components/Home'

export default {
  name: 'App',
  components: {
    Home
  }
}
</script>

<style>
body {
  background-color: #2c3e50;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
  margin-top: 60px;
}
.cryptopia-logo {
  width: 100%;
  max-width: 300px;
}
</style>
