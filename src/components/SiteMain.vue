<template lang="html">
  <div id="main-page">
    <beer-map :filteredBeers="filteredBeers"/>
    <map-overlay
      v-if="selectedBrewery"
      :selectedBrewery="selectedBrewery" />
    </div>
  </div>
</template>

<script>
import BeerDetails from './BeerDetails.vue'
import BeerList from './BeerList.vue'
import BeerMap from './BeerMap.vue'
import MapOverlay from './MapOverlay.vue'
import { eventBus } from '../main.js'

export default {
  name: 'main-page',
  props: ['filteredBeers'],
  data() {
    return {
      selectedBrewery: null
    }
  },
  components: {
    'beer-details': BeerDetails,
    'beer-list': BeerList,
    'beer-map': BeerMap,
    'map-overlay': MapOverlay,
  },
  mounted() {
    eventBus.$on('selected-brewery', brewery => { this.selectedBrewery = brewery })
    eventBus.$on('overlay-close', () => this.selectedBrewery = null)
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
