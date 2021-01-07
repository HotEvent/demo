<template>
  <div id="app">
    <!-- css版 -->
    <FireflyCss/>  
    <!-- 插件版 -->
    <!-- <FireflyTweenMax/> -->
  </div>
</template>

<script>
/* eslint-disable */
import FireflyCss from './components/FireflyCss.vue'
import FireflyTweenMax from './components/FireflyTweenMax.vue'

export default {
  name: 'App',
  components: {
    FireflyCss,
    FireflyTweenMax
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
