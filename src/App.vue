<template>
  <div id="app">
    <!-- <HelloWorld /> -->
    <Gauge />
    <Echarts />
    <Bargraph />
  </div>
</template>

<script>
import Echarts from './components/Echarts.vue'
import Gauge from './components/Gauge.vue'
import Bargraph from './components/Bargraph.vue'


export default {
  name: 'App',
  components: {
    Gauge,
    Echarts,
    Bargraph
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
  display: flex;
  flex-direction: column;
  justify-content: center;
}
</style>
