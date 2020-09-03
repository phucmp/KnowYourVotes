<template>
    <div>
        <checkbox-svg-map v-model="selectedLocations" :map="customUSA" />
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
                label: "US MAP"
            },
            selectedLocations: []
        }
    }
}
</script>

<style src="vue-svg-map/dist/index.css"></style>