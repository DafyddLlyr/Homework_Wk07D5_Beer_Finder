<template lang="html">
  <div id="map-overlay">
    <div class="details-popup">
      <i class="fas fa-times" v-on:click="closeOverlay"></i>
      <brewery-details v-if="selectedBrewery" :beer="selectedBeer.fields"/>
      <country-details v-if="selectedCountry" :beer="selectedBeer.fields"/>
    </div>
  </div>
</template>

<script>
import BreweryDetails from './BreweryDetails.vue'
import CountryDetails from './CountryDetails.vue'
import { eventBus } from '../main.js'

export default {
  name: 'map-overlay',
  props: ['selectedBrewery', 'selectedBeer', 'selectedCountry'],
  components: {
    'brewery-details': BreweryDetails,
    'country-details': CountryDetails,
  },
  methods: {
    closeOverlay: function() {
      eventBus.$emit('overlay-close', this.selectedBrewery);
    }
  }
}
</script>

<style lang="css" scoped>

#map-overlay {
  position: fixed;
  z-index: 2000;
  bottom: 0;
  right: 0;
  width: 70vw;
  height: 90vh;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
}

.details-popup {
  background-color: white;
  width: 60%;
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.fa-times {
  align-self: flex-end;
  margin: 1vw;
  color: #c3c3c3;
  font-size: 16px;
  cursor: pointer;
  z-index: 9000;
}

.fa-times:hover {
  color: #F06543;
}

</style>
