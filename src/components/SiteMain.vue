<template lang="html">
  <div id="main-page">
    <beer-map :filteredBeers="filteredBeers"/>
    <map-overlay
      v-if="selectedBrewery || selectedCountry || selectedStyle"
      :selectedBeer="selectedBeer"
      :selectedCountry="selectedCountry"
      :selectedStyle="selectedStyle"
      :selectedBrewery="selectedBrewery"/>
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
      selectedBrewery: null,
      selectedBeer: null,
      selectedCountry: null,
      selectedStyle: null
    }
  },
  components: {
    'beer-details': BeerDetails,
    'beer-list': BeerList,
    'beer-map': BeerMap,
    'map-overlay': MapOverlay,
  },
  mounted() {
    eventBus.$on('selected-brewery', beer => {
      this.selectedBrewery = beer.fields.name_breweries;
      this.selectedBeer = beer;
    })
    eventBus.$on('selected-country', beer => {
      this.selectedCountry = beer.fields.country;
      this.selectedBeer = beer;
    })
    eventBus.$on('selected-style', beer => {
      this.selectedStyle = beer.fields.style_name;
      this.selectedBeer = beer;
    })
    eventBus.$on('overlay-close', () => {
      this.selectedBrewery = null
      this.selectedCountry = null
      this.selectedStyle = null
    })
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
