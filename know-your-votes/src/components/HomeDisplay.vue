<template>
    <div>
        <div class="vh-100 d-flex flex-column justify-content-md-center align-items-center home" ref="title">
            <div>
                <h1 class="title">Know Your Votes</h1>
            </div>
            <div>
                <h4 class="subtitle">Presidential Interactive Map for the 2020 Election</h4>
            </div>
            <div class="mt-5">
                <b-button v-on:click="scrollTo('map')" variant="warning" size="lg">Get Started</b-button>
            </div>
        </div>
        <div class="vh-100" ref="map">
            <h1>Map Wrapper</h1>
        </div>
    </div>
</template>

<script>
export default {
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
    background-image: url('~@/assets/bg.jpg');
    background-size: cover;
}
.title {
    font-size: 100px;
    color: black;
}
.subtitle{
    color: black;
}
</style>