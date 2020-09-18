<template>
  <div class="vh-100 container-fluid">
    <div class="mt-5">
      TOTAL REPUBLICAN ELECTORAL VOTE: {{totalVotes.rep}}
      TOTAL DEMOCRAT ELECTORAL VOTE: {{totalVotes.dem}}
    </div>
    <svg class="map mt-3"></svg>
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
      var svg = d3.select('.map').attr('viewBox', "-100 0 1100 550")
      var projection = d3.geoAlbersUsa();
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
            vm.setParty(this, vm, d);
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
          .attr("dx", function(d){
            return d.properties.DX || 0;
          })
          .attr("y", function(d){
              return  path.centroid(d)[1];
          })
          .attr("dy", function(d){
            return d.properties.DY || 0;
          })
          .attr("class", "state-text")
          .append('svg:tspan')
          .attr("x", function(d){
              return path.centroid(d)[0];
          })
          .attr("dx", function(d){
            return d.properties.DX || 0;
          })
          .attr('dy', 15)
          .text(function(d) { 
            return d.properties.VOTES; 
          });
      });
    },
    setParty(elem, vm, d) {
      if (elem.classList.contains('democrat')) {
        elem.classList.remove('democrat');
        elem.classList.add('republican');
        vm.totalVotes.dem -= d.properties.VOTES;
        vm.totalVotes.rep += d.properties.VOTES;
      } else if (elem.classList.contains('republican')) {
        elem.classList.remove('republican');
        vm.totalVotes.rep -= d.properties.VOTES;
      } else {
        elem.classList.add('democrat');
        vm.totalVotes.dem += d.properties.VOTES;
      }
    }
  }
}
</script>

<style>
.map {
  width: 100%;
  height: auto;
  stroke: #fff;
  stroke-width: 1;
  stroke-linecap: round;
  stroke-linejoin: round; 
}
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
.state-text {
  pointer-events: none;
  text-anchor: middle;
  stroke: black;
  font-weight: lighter;
  fill: none;
  font-size: 7pt;
}

@media only screen and (max-width: 1015px) {

}

@media only screen and (max-width: 700px) {

}

@media only screen and (max-width: 400px) {

}
</style>