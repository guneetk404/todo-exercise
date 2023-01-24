<template>

  <TodoList></TodoList>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import TodoList from './components/TodoList.vue'
export default {
  name: 'App',
  components: {
    HelloWorld,
    TodoList,
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

.container {
  max-width: 600px;
  margin: 0 auto;
}


#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  display: flex;
  justify-content: center;
  color: #2c3e50;
  margin-top: 100px;
}
</style>
