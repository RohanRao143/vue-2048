<template>
  <div id="app">
    <b-card class="text-center">
      <h1>2048</h1>
    </b-card>
    <Game />
  </div>
</template>

<script>
import Game from './components/Game.vue'

export default {
  name: 'app',
  components: {
    Game
  }
}
</script>

<style>
.text-center{
  margin: 25px 520px 15px 520px;
  background: #f18a8a;
}
#app {
  width:100%;
  background-color: #a7caef;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
