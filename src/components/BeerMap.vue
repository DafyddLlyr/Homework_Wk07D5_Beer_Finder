<template>
  <l-map style="height: 100%; width: 100%" :zoom="zoom" :center="center" ref="beerMap" :maxZoom="10" :minZoom="2">
    <l-tile-layer :url="url"></l-tile-layer>
    <l-feature-group
      layerType="overlay"
      name="BeerMarkers">
    <l-marker
      v-for="(beer, index) in filteredBeers"
      :key="index"
      :ref="beer.fields.id"
      :lat-lng="[beer.fields.coordinates[0], beer.fields.coordinates[1]]">
    <l-popup >
      <beer-details :beer="beer"/>
    </l-popup>
  </l-marker>
</l-feature-group>
</l-map>
</template>

<script>
import { keys } from '../../private/apikeys.js'
import { eventBus } from '../main.js'
import BeerDetails from './BeerDetails.vue'

export default {
  name: 'beer-map',
  props: ['filteredBeers'],
  data () {
    return {
      selectedBeer: null,
      beerMap: null,
      markers: null,
      url: `https://api.tiles.mapbox.com/v4/mapbox.streets/{z}/{x}/{y}.png?access_token=${keys['leaflet']}`,
      zoom: 10,
      center: [55.8653523,-4.2598484]
    }
  },
  components: {
    'beer-details': BeerDetails
  },
  computed: {
    filteredBeersBounds: function() {
      return this.filteredBeers.map(beer => beer.fields.coordinates)
    },
    // icon: function() {
    //   return L.icon({
    //     iconUrl: url,
    //     iconSize: [40, 40],
    //     iconAnchor: [20, 20]
    //   })
    // }
  },
  watch: {
    filteredBeers: function() {
      this.$refs.beerMap.mapObject.flyToBounds(this.filteredBeersBounds);
    }
  },
  mounted() {
    eventBus.$on('random-beer', beer => {
      const adjustedCoords = [beer.fields.coordinates[0] + 0.1, beer.fields.coordinates[1]]
      this.$refs.beerMap.mapObject.flyTo(adjustedCoords, 10);
      this.selectedBeer = beer;

      let selectedBeerRef = this.selectedBeer.fields.id;
      this.$refs[selectedBeerRef][0].mapObject.openPopup()
    }
  )
}
}
</script>
