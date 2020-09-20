<template>
  <div class="vh-100 container-fluid">
    <div class="mt-5">
      TOTAL REPUBLICAN ELECTORAL VOTE: {{totalVotes.rep}}
      TOTAL DEMOCRAT ELECTORAL VOTE: {{totalVotes.dem}}
    </div>
    <svg class="map mt-3"></svg>
    <div>
      <p>Smaller States Buttons</p>
      <div class="container">
        <div class="row m-auto">
          <div v-for="(state, index) in states" :key="index" class="col-lg-2">
            <p @click="triggerStateParty" :id="state.id" class="small-state"><b>{{state.abbr}}</b><br>{{state.votes}}</p>
          </div>
        </div>
      </div>
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
      },
      smallStates: [ "MA", "RI", "CT", "NJ", "DE", "MD" ],
      states: [
        {
          id: "ma",
          abbr: "MA",
          votes: 11
        },
        {
          id: "ri",
          abbr: "RI",
          votes: 4
        },
        {
          id: "ct",
          abbr: "CT",
          votes: 7
        },
        {
          id: "nj",
          abbr: "NJ",
          votes: 14
        },
        {
          id: "de",
          abbr: "DE",
          votes: 3
        },
        {
          id: "md",
          abbr: "MD",
          votes: 10
        }
      ]
    }
  },
  mounted: function() {
    this.display();
  },
  methods: {
    display() {
      var vm = this;
      var svg = d3.select('.map').attr('viewBox', "-75 0 1100 550")
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
          .attr("id", function(d) {
            return d.properties.STATE_ABBR
          })
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
            if (vm.smallStates.indexOf(d.properties.STATE_ABBR) === -1) {
              return d.properties.VOTES; 
            }
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
    },
    triggerStateParty(event) {
      document.getElementById(event.target.id.toString().toUpperCase()).dispatchEvent(new Event('click'));
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
.small-state {
  border-radius: 25px;
  border-style: solid;
  border-width: thin;
  color: black;
  border-color: #ccc;
  background-color: #ccc;
  font-size: 11pt
}
.small-state:hover {
  opacity: 50%;
  cursor: pointer;
}
</style>