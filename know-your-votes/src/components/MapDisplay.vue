<template>
    <div>
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
            selectedLocations: []
        }
    },
    methods: {
      onClick(event) {
        console.log(event);
      }
    },
    beforeUpdate: function() {
      alert('hi');
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
.svg-map__location:focus, .svg-map__location:hover {
  /* fill: #1494E9; */
  fill: #ED3537;
  outline: 0; 
}
.svg-map__location[aria-checked="true"] {
  fill: #1494E9; 
}
</style>