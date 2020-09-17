<template>
  <div class="chart">
    <div class="bar-1"></div>
    <div class="bar-2"></div>
    <div class="bar-3"></div>
    <div class="bar-4"></div>
    <div class="bar-5"></div>
    <div class="bar-6"></div>
    <div class="bar-7"></div>
    <div class="bar-8"></div>
    <div class="bar-9"></div>
    <div class="bar-10"></div>
    <div class="bar-11"></div>
    <div class="bar-12"></div>    
  </div>
</template>

<script>
export default {
  name: 'barGraph',
  data() {
    return {
      header: ["Name", "Death toll"],
      rows: []      
    }
  },
  mounted() {
  },
  methods: {
  }  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.chart {
  height: 200px;
  width: 800px;
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  grid-template-rows: repeat(1, 1, 1, 1fr);
  grid-column-gap: 15px;
}
[class*="bar"] {
  grid-row-start: 1;
  grid-row-end: 101;
  border-radius: 5px 5px 0 0;
  background-color: pink;
}

[class*="bar"]:nth-child(odd) {
  background-color: #ff4136; 
}

[class*="bar"]:nth-child(even) {
  background-color: #0074d9;
}
</style>
