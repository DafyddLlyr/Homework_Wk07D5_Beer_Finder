<template>
  <l-map style="height: 100%; width: 100%" :zoom="zoom" :center="center">
    <l-tile-layer :url="url"></l-tile-layer>
    <l-layer-group
      layerType="overlay"
      name="BeerMarkers">
      <l-marker
           v-for="(beer, index) in filteredBeers"
           :key="index"
           :lat-lng="[beer.fields.coordinates[0], beer.fields.coordinates[1]]"
      >
        <l-popup><beer-details :beer="beer"/></l-popup>
      </l-marker>
  </l-layer-group>
  </l-map>
</template>

<script>
import { keys } from '../../private/apikeys.js'
import BeerDetails from './BeerDetails.vue'


export default {
  name: 'beer-map',
  props: ['filteredBeers'],
  data () {
    return {
      beerMap: null,
      markers: null,
      url: `https://api.tiles.mapbox.com/v4/mapbox.streets/{z}/{x}/{y}.png?access_token=${keys['leaflet']}`,
      zoom: 5,
      center: [47.413220, -1.319482],
      markerLatLng: [47.313220, -1.319482]
    }
  },
  computed: {
    bounds: function() {

    }
  },
  components: {
    'beer-details': BeerDetails
  }
}
</script>

<!-- <template lang="html">


  watch: {
    filteredBeers: function() {
      if (this.markers !== null) { this.markers.clearLayers() }
      let markerArray = this.filteredBeers.map(beer => {

        return L.marker(beer.fields.coordinates)
          .bindPopup(beerDetails).openPopup();
      })
      this.markers = L.featureGroup(markerArray)
      this.beerMap.addLayer(this.markers);
      this.beerMap.flyToBounds(this.markers.getBounds());
    }
  }

<style lang="css">


</style> -->
