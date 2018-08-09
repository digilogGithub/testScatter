<template>
  <div>
    <svg id="myScatter" v-bind="initScatter()">
      <!-- <svg id="myScatter"> -->
    </svg>
  </div>
</template>

<script>
  export default {

    data() {
      return {
        dataSet:[],
        svgWidth: 1200,
        svgHeight: 1020,
        // dataSet:[[10,20],[30,40]]
      }
    },

    created() {
      d3.dsv(",","src/dataset/cdData.csv").then((data)=>{
        for(var i=0;i<data.length;i++) {
          this.dataSet.push([data[i].date, data[i].value])
        }

      })
    },

    mounted() {
      setInterval(()=>{ this.dataSet = this.updateData(this.dataSet); this.updataGraph()},2000)
    },

    methods: {
      initScatter() {
        d3.select("#myScatter").selectAll("circle")
          .data(this.dataSet)
          .enter()
          .append("circle")
          .attr("class", "mark")
          .attr("cx",(d,i) => { return d[0]})
          .attr("cy",(d,i) => { return this.svgHeight-d[1]})
          .attr("r",2)

      },
      updateData(dataSet) {
        return dataSet.map((d,i)=>{
          var x = Math.random() * (this.svgWidth-10);
          var y = Math.random() * (this.svgHeight-10);
          return [x,y]
        })

      },

      updataGraph() {
        d3.select("#myScatter").selectAll("circle")
          .data(this.dataSet)
          .transition()
          .attr("cx",(d,i) => { return this.svgWidth-d[0]})
          .attr("cy",(d,i) => { return this.svgHeight-d[1]})
          .attr("r",2)
      },
    }

  }
</script>

<style>
  svg {
    width: 1220px;
    height: 1040px;
    border: 1px solid black;
  }
  .mark {
    fill: cornflowerblue;
    stroke: none;
  }
</style>
