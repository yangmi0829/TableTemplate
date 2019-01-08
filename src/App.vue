<template>
  <div id="app">
    <table-demo/>
  </div>
</template>

<script>
import TableDemo from './components/TableDemo.vue'

export default {
  name: 'app',
  components: {
    TableDemo
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
