<template>
  <div>
    <svg/>
    <div>
      TOTAL REPUBLICAN ELECTORAL VOTE: {{totalVotes.dem}}
      TOTAL DEMOCRAT ELECTORAL VOTE: {{totalVotes.rep}}
    </div>
  </div>
</template>

<script>
import * as d3 from 'd3'
import * as topojson from "topojson-client";

export default {
  data() {
    return {
      totalVotes: {
        dem: 0,
        rep: 0
      }
    }
  },
  mounted: function() {
    this.display();
  },
  methods: {
    display() {
      var vm = this;
      var width = 1200;
      var height = 800;
      console.log(this.$el.firstElementChild);
      var svg = d3.select(this.$el.firstElementChild).attr('width', width).attr('height', height);
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
          .on('click', function(event, d) {
            if (this.classList.contains('democrat')) {
              this.classList.remove('democrat');
              this.classList.add('republican');
              vm.totalVotes.dem -= d.properties.VOTES;
              vm.totalVotes.rep += d.properties.VOTES;
            } else if (this.classList.contains('republican')) {
              this.classList.remove('republican');
              vm.totalVotes.rep -= d.properties.VOTES;
            } else {
              this.classList.add('democrat');
              vm.totalVotes.dem += d.properties.VOTES;
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
              return path.centroid(d)[0] + ( d.properties.DX || 0 );
          })
          .attr("y", function(d){
              return  path.centroid(d)[1] + ( d.properties.DY || 0 );
          })
          .attr("pointer-events", "none")
          .attr("text-anchor","middle")
          .attr("stroke", "black")
          .attr("fill", "none")
          .attr('font-size','10pt')
          .append('svg:tspan')
          .attr("x", function(d){
              return path.centroid(d)[0] + ( d.properties.DX || 0 );
          })
          .attr('dy', 15)
          .text(function(d) { 
            return d.properties.VOTES; 
          });
      });
    }
  }
}
</script>

<style>
.state {
  fill: #ccc;
  stroke: #fff;
  stroke-width: 3;
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