<template>
  <svg/>
</template>

<script>
import * as d3 from 'd3'
import * as topojson from "topojson-client";

export default {
  mounted: function() {
    this.display();
  },
  methods: {
    display() {
      var width = 1200;
      var height = 800;
      var svg = d3.select(this.$el).attr('width', width).attr('height', height);
      var projection = d3.geoAlbersUsa().scale(1500).translate([width / 2, height / 2]);
      var path = d3.geoPath().projection(projection);

      d3.json("us.json").then(function(response) {
        var group = svg.append('g');

        group.selectAll('.state')
          .data(topojson.feature(response, response.objects.usStates).features)
          .enter()
          .append("path")
          .attr("class", "state")
          .attr("d", path)
          .on('click', function(d) {
            if (this.classList.contains('democrat')) {
              this.classList.remove('democrat');
              this.classList.add('republican');
            } else if (this.classList.contains('republican')) {
              this.classList.remove('republican');
            } else {
              this.classList.add('democrat');
            }
          });

        group.selectAll("text")
          .data(topojson.feature(response, response.objects.usStates).features)
          .enter()
          .append("svg:text")
          .text(function(d){
            return d.properties.STATE_ABBR;
          })
          .attr("x", function(d){
              return path.centroid(d)[0];
          })
          .attr("y", function(d){
              return  path.centroid(d)[1];
          })
          .attr("pointer-events", "none")
          .attr("text-anchor","middle")
          .attr("stroke", "black")
          .attr("fill", "none")
          .attr('font-size','10pt');
      });
    }
  }
}
</script>

<style>
.state {
  fill: #ccc;
  stroke: #fff;
}
.state:hover {
  opacity: 50%;
  cursor: pointer; 
}
.democrat {
  fill: #1494E9;
}
.republican {
  fill: #ED3537;
}
</style>