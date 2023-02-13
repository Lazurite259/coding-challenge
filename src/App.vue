<template>
  <div>
    <Breweries />
  </div>
</template>

<script>
import Breweries from './components/Breweries.vue'
export default {
  name: 'App',
  components: {
    Breweries
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
