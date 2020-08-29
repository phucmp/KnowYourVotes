<template>
  <div class="home">
    <HomeDisplay/>
  </div>
</template>

<script>
// @ is an alias to /src
import HomeDisplay from '@/components/HomeDisplay.vue'

export default {
  name: 'Home',
  components: {
    HomeDisplay
  }
}
</script>
