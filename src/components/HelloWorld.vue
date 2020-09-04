<template>
  <div class="hello">
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>
    <div id="arc"></div>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  name: 'HelloWorld',
  data() {
    return {
      gdp: [
        {country: "USA", value: 20.5 },
        {country: "China", value: 13.4 },
        {country: "Germany", value: 4.0 },
        {country: "Japan", value: 4.9 },
        {country: "France", value: 2.8 }
      ]
    }
  },
  mounted() {
    this.generateArc();
  },
  methods: {
      generateArc() {
        const w = 500;
        const h = 500;

        const svg = d3
          .select("#arc")
          .append("svg")
          .attr("width", w)
          .attr("height", h);

        const sortedGDP = this.gdp.sort((a, b) => (a.value > b.value ? 1 : -1));
        const color = d3.scaleOrdinal(d3.schemeDark2);

        const max_gdp = d3.max(sortedGDP, o => o.value);

        const angleScale = d3
          .scaleLinear()
          .domain([0, max_gdp])
          .range([0, 1.5 * Math.PI]);

        const arc = d3
          .arc()
          .innerRadius((d, i) => (i + 1) * 25)
          .outerRadius((d, i) => (i + 2) * 25)
          .startAngle(angleScale(0))
          .endAngle(d => angleScale(d.value));

        const g = svg.append("g");

        g.selectAll("path")
          .data(sortedGDP)
          .enter()
          .append("path")
          .attr("d", arc)
          .attr("fill", (d, i) => color(i))
          .attr("stroke", "#FFF")
          .attr("stroke-width", "1px")
          .on("mouseenter", function() {
            d3.select(this)
              .transition()
              .duration(200)
              .attr("opacity", 0.5);
          })
          .on("mouseout", function() {
            d3.select(this)
              .transition()
              .duration(200)
              .attr("opacity", 1);
          });

        // g.selectAll("text")
        //   .data(this.gdp)
        //   .enter()
        //   .append("text")
        //   .text(d => `${d.country} -  ${d.value} Trillion`)
        //   .attr("x", -150)
        //   .attr("dy", -8)
        //   .attr("y", (d, i) => -(i + 1) * 25);

        g.attr("transform", "translate(200,300)");
    }
  }  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
