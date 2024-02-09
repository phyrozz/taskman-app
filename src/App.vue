<template>
  <div id="app">
    <TaskManager />
  </div>
</template>

<script>
import TaskManager from './components/TaskManager.vue'

export default {
  name: 'App',
  components: {
    TaskManager
  }
}
</script>

<style>
body {
  font-family: "Roboto Slab", serif;
  background-color: #E8EAF6;
}
</style>
