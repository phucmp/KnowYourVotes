<template>
    <div class="container">
        <checkbox-svg-map v-model="selectedLocations" :map="customUSA" @click="onClick" />
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
            selected: []
        }
    },
    methods: {
      onClick(event) {
        if (event.target.classList.contains('democrat')) {
          event.target.classList.remove('democrat');
          event.target.classList.add('republican');
        } else if (event.target.classList.contains('republican')) {
          event.target.classList.remove('republican');
        } else {
          event.target.classList.add('democrat');
        }
      }
    },
    beforeUpdate: function() {
      
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