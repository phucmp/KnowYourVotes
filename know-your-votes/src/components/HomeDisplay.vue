<template>
    <div>
        <div class="vh-100 d-flex flex-column justify-content-center home" ref="title">
            <div class="div-wrapper d-flex flex-column">
                <div class="title-wrapper mr-auto ml-5">
                    <h1 class="title">Know Your Votes</h1>
                </div>
                <div class="title-wrapper mr-auto ml-5">
                    <h4>Presidential Interactive Map for the 2020 Election</h4>
                </div>
                <div class="button-wrapper mr-auto ml-5 mt-5">
                    <b-button class="xl-button" v-on:click="scrollTo('map')" variant="info" size="lg">Get Started</b-button>
                </div>
            </div>
        </div>
        <div class="vh-100" ref="map">
            <map-display/>
        </div>
    </div>
</template>

<script>
import MapDisplay from './MapDisplay.vue'
export default {
    components: {
        MapDisplay
    },
    methods: {
        scrollTo: function(refName) {
            this.$nextTick(() => {
                console.log(this.$refs[refName]);
                this.$refs[refName].scrollIntoView();
            })
        }
    }
}
</script>

<style scoped>
.home {
    background-image: url('~@/assets/votebg.jpg');
    background-size: cover;
}

.div-wrapper {
    margin-top: -150px;
}

.title-wrapper{
    color: white;
}

.title {
    font-size: 100px;
    font-family: Impact, Charcoal, sans-serif;
}

.xl-button {
    line-height: 2.5;
    width: 200px;
}

@media only screen and (max-width: 1015px) {
    .title-wrapper {
        background-color: rgba(0, 0, 0, 0.5);
    }
}

@media only screen and (max-width: 700px) {
    .title-wrapper {
        margin: 0 !important;
    }
    .title-wrapper > h4 {
        font-size: 15px;
    }
    .title {
        font-size: 50px;
    }
    .button-wrapper {
        margin: 0 !important;
        margin-top: 25px !important;
    }
}

@media only screen and (max-width: 400px) {
    .home{
        background-position-x: -100px;
    }
}
</style>