<template>
  <div id="app">
    
    <therps/>
  </div>
</template>

<script>
import therps from './components/therps.vue'

export default {
  name: 'app',
  components: {
    therps
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
