<template lang="html">
  <div id="main-page">
    <h2>Main</h2>
    <beer-details />
  </div>
</template>

<script>
import BeerDetails from './BeerDetails.vue'

export default {
  name: 'main-page',
  props: ['allBeers'],
  components: {
    'beer-details': BeerDetails
  }
}
</script>

<style lang="css" scoped>

#main-page {
  display: flex;
  align-items: center;
  flex-direction: column;
  background-color: lightgrey;
  grid-area: main;
}

</style>
