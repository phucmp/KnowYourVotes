<template>
    <div>
        <div class="vh-100 border" ref="title">
            <h1>Title Wrapper</h1>
            <b-button v-on:click="scrollTo('map')" variant="outline-dark">Get Started</b-button>
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
</style>