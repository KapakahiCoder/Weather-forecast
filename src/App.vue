<template>
  <div id="app">
    <GetLocation />
    <Weather />
  </div>
</template>

<script>
import GetLocation from './components/GetLocation.vue'
import Weather from './components/Weather.vue'

export default {
  name: 'App',
  components: {
    GetLocation,
    Weather
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
