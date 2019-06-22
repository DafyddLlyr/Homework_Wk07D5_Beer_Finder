<template lang="html">
  <div id="main-page">
    <beer-map :filteredBeers="filteredBeers"/>
    <!-- <beer-list :filteredBeers="filteredBeers"/> -->
    <!-- <beer-details /> -->
  </div>
</template>

<script>
import BeerDetails from './BeerDetails.vue'
import BeerList from './BeerList.vue'
import BeerMap from './BeerMap.vue'

export default {
  name: 'main-page',
  props: ['filteredBeers'],
  components: {
    'beer-details': BeerDetails,
    'beer-list': BeerList,
    'beer-map': BeerMap
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
