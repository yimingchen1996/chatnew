<template>
  <div>
    <h1>艾米的ChatGPT壳子</h1>
    <ChatGPT />
  </div>
</template>


<script>
import ChatGPT from './components/HelloWorld.vue'

export default {
  components: {
    ChatGPT
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
</style>
