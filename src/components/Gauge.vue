<template>
  <div class="gauge">
    <h3>{{customerConnection.title}}</h3>
    <div id="gauge"></div>
    <h5>{{customerConnection.chartName}}</h5>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  name: 'Gauge',
  data() {
    return {
      customerConnection: {
        oldF: 73,
        newF: 50,
        title: 'Customer Connection',
        chartName: 'Sample N=X'
      },
      storeOperation: {
        oldF: 70,
        newF: 60,
        title: 'Store Operation',
        chartName: 'Sample N=X'
      }      
    }
  },
  mounted() {
    this.generateArc();
  },
  methods: {
    generateArc() {
      const width = 200;
      const height = 200;
      const innerRadius = 100;
      const outerRadius = 65;
      const pi = Math.PI;
      let startColor = 'limegreen',
          emptyColor = 'f8f8f8',
          oldColor = '#00633f',
          newColor = '#21936d';
      if(this.customerConnection.newF - this.customerConnection.oldF < 0) newColor="#ececec";

      const svg = d3.select("#gauge")
                    .append("svg")
                    .attr("width", width)
                    .attr("height", height);

      const arc = d3.arc()
                    .innerRadius(innerRadius)
                    .outerRadius(outerRadius)
                    .startAngle(-90 * (pi / 180));

      const g = svg.append("g").attr("transform", `translate(${width / 2}, ${height / 2})`);

      // Set default background color
      g.append("path")
        .datum({ endAngle: 90 * (pi / 180) })
        .style("fill", emptyColor)
        .attr("d", arc);

      // Set old color
      const oldBackground = g.append("path")
                          .datum({ endAngle: -90 * (pi / 180) })
                          .style("fill", oldColor)
                          .attr("d", arc);   

      // Display Current value Text
      svg.append("text")
          .attr("transform", `translate(90, ${innerRadius})`)
          .attr("text-anchor", "middle")
          .style("font-size", "40")
          .style("font-weight", "700")
          .style("font-family", "Helvetica")
          .text(this.customerConnection.newF)

      // Display Percentage Text
      svg.append("text")
          .attr("transform", `translate(127, ${innerRadius})`)
          .attr("text-anchor", "middle")
          .style("font-size", "30")
          .style("font-weight", "700")
          .style("font-family", "Helvetica")
          .text('%')

      const oldNum = this.customerConnection.oldF / 100 * 180;
      const oldNumPi = Math.floor(oldNum - 89) * (pi / 180); 

      const newNum = this.customerConnection.newF / 100 * 180;
      const newNumPi = Math.floor(newNum - 89) * (pi / 180);

      // Set new Arc for new data
      const arc2 = d3.arc()
                     .innerRadius(innerRadius)
                     .outerRadius(outerRadius)
                     .startAngle(oldNumPi);      

      // Set new color
      const newBackground = g.append("path")
                             .datum({ endAngle: -90 * (pi / 180) })
                             .style("fill", newColor)
                             .attr("d", arc2);     

      // Update animation
      const arcTween = (transition, newAngle) => { 
        transition.attrTween("d", d => {
          const interpolate = d3.interpolate(d.endAngle, newAngle);
          return t => {
            d.endAngle = interpolate(t);
            return arc(d);
          };
        });       
      }

      const customArcTween = (transition, newAngle) => { 
        transition.attrTween("d", d => {
          const interpolate = d3.interpolate(oldNumPi, newAngle);
          return t => {
            d.endAngle = interpolate(t);  
            return arc2(d);
          };
        });       
      }                      

      // Arc Transition
      oldBackground
        .transition()
        .duration(500)
        .styleTween("fill", function() { 
          return d3.interpolate(startColor, oldColor);
        })
        .call(arcTween, oldNumPi);

      newBackground
        .transition()
        .duration(3000)
        .styleTween("fill", function() { 
          return d3.interpolate(oldColor, newColor);
        })
        .call(customArcTween, newNumPi);
    }
  }  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.gauge {
  position: relative;
  background-color: white;
  box-shadow: 0 2px 20px 0 rgba(0,0,0,0.10);
  border-radius: 30px;
  display: flex;
  justify-content: center;
  flex-direction: column;
  padding: 30px;
  width: 200px;
  height: 200px;
}
#gauge {
  position: absolute;
  top: 27%;
}
h3 {
  position: absolute;
  top: 5%;
  left: calc(50% - 100px);
}
h5 {
  position: absolute;
  bottom: 3%;
  left: calc(50% - 50px);
}
</style>
