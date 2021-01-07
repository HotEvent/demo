<template>
  <div id="app">
    <FireflyCss/>
  </div>
</template>

<script>
import FireflyCss from './components/FireflyCss.vue'

export default {
  name: 'App',
  components: {
    FireflyCss
  }
}
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
body{
  min-height: 100vh;
  background: black;
}
</style>
