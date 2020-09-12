<template>
    <div class="container">
        <checkbox-svg-map v-model="selectedLocations" :map="customUSA" @click="onClick">
          <template v-slot:after>
            <text v-for="(state, index) in states" :key="index" style="font-size: 24px;">
              <textPath :xlink:href="'#'+state" startOffset="50%" text-anchor="middle">{{state}}</textPath>
            </text>
          </template>
        </checkbox-svg-map>
    </div>
</template>

<script>
import { CheckboxSvgMap } from "vue-svg-map";
import USA from "@svg-maps/usa";

export default {
    name: "MyMap",
    components: {
        CheckboxSvgMap
    },
    data() {
        return {
            customUSA: {
                ...USA,
                label: "US MAP",
                // locations: USA.locations.map(location => {
                //   // Modify each location to customize/add attributes of <path>
                // })
            },
            selectedLocations: [],
            states: [ "al", "ak", "az", "ar", "ca", "co", "ct", "dc", "de", "fl", "ga", "hi", "id", "il", "in", "ia", "ks", "ky", "la", "me", "md", "ma", "mi", "mn", "ms", "mo", "mt", "ne", "nv", "nh", "nj", "nm", "ny", "nc", "nd", "oh", "ok", "or", "pa", "ri", "sc", "sd", "tn", "tx", "ut", "vt", "va", "wa", "wv", "wi", "wy"]
        }
    },
    methods: {
      onClick(event) {
        console.log(event.target.parentElement);
        if (event.target.classList.contains('democrat')) {
          event.target.classList.remove('democrat');
          event.target.classList.add('republican');
          event.target.innerHTML = "click";
        } else if (event.target.classList.contains('republican')) {
          event.target.classList.remove('republican');
        } else {
          event.target.classList.add('democrat');
        }
      }
    }
}
</script>

<style>
.svg-map {
  width: 100%;
  height: auto;
  stroke: #fff;
  stroke-width: 1;
  stroke-linecap: round;
  stroke-linejoin: round; 
}
.svg-map__location {
  fill: #bdbec7;
  cursor: pointer; 
}
.svg-map__location:hover {
  /* fill: #1494E9; */
  fill: black !important;
}
.svg-map__location:focus {
  outline: 0; 
}
.democrat {
  fill: #1494E9 !important;
}
.republican {
  fill: #ED3537 !important;
}
</style>