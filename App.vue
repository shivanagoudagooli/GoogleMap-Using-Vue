<template>
  <div id="app">
    <google-map name="example"></google-map>
  </div>
</template>

<script>
import GoogleMap from './components/Map.vue'

 export default {
  name: 'app',
  components: {
    GoogleMap,
  
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
  margin-top: 20px;
}
</style>