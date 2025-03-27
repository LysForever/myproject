<template>
  <div id="app">
    <BasketballGame />
  </div>
</template>

<script>
import BasketballGame from './components/BasketballGame.vue'

export default {
  name: 'App',
  components: {
    BasketballGame
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: #f0f0f0;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
