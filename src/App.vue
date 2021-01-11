<template>
  <div id="app">
    <!-- css版 -->
    <!-- <FireflyCss/>   -->
    <!-- 插件版 -->
    <!-- <FireflyTweenMax/> -->
    <Chat/>
  </div>
</template>

<script>
/* eslint-disable */
import FireflyCss from './components/FireflyCss.vue'
import FireflyTweenMax from './components/FireflyTweenMax.vue'
import Chat from "./components/Chat.vue";
export default {
  name: 'App',
  components: {
    FireflyCss,
    FireflyTweenMax,
    Chat
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
  /* background: black; */
}
</style>
