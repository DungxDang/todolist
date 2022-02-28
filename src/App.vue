<template>
  <h1>TODOS</h1>
  <Todos/>
</template>

<script>
import Todos from './components/Todos.vue'

export default {
  name: 'App',
  components: {
    Todos
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
h1 {
  color: rebeccapurple;
  font-size: 3.3em;
  opacity: .7;
}
body {
  display: flex;
  justify-content: center;
}
</style>
