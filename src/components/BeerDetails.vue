<template lang="html">
  <div id="beer-details">
    <p>Beer Details</p>
    <p> {{ beer.fields.name }}</p>
    <p> {{ beer.geometry.coordinates }}</p>
  </div>
</template>

<script>
import { eventBus } from '../main.js'
export default {
  name: 'beer-details',
  data() {
    return {
      beer: ''
    }
  },
  mounted() {
    eventBus.$on('selected-beer', beer => this.beer = beer)
  }
}
</script>

<style lang="css" scoped>

#beer-details {
  background-color: white;
  width: 90%;
  margin-top: 2vw;
}

</style>
