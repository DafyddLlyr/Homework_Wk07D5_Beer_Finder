<template lang="html">
  <div class="container">
    <p v-on:click="createMarkers">Add</p>
    <p v-on:click="removeMarkers">Remove</p>
    <div id="beer-map">

    </div>
  </div>

</template>

<script>
import { keys } from '../../private/apikeys.js'

export default {
  name: 'beer-map',
  props: ['filteredBeers'],
  data() {
    return {
      beerMap: null,
      markers: null
    }
  },
  watch: {
    filteredBeers: function() {
      if (this.markers !== null) { this.markers.clearLayers() }
      let markerArray = this.filteredBeers.map(beer => {
        return L.marker(beer.fields.coordinates)
      })
      this.markers = L.featureGroup(markerArray)
      this.beerMap.addLayer(this.markers);
      this.beerMap.flyToBounds(this.markers.getBounds());
    }
  },
  mounted() {
    this.beerMap = L.map('beer-map').setView([51.505, -0.09], 5 );
    L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}', {
      attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
      maxZoom: 18,
      id: 'mapbox.streets',
      accessToken: keys['leaflet']
    }).addTo(this.beerMap);
  }
}
</script>

<style lang="css" scoped>

#beer-map, .container {
  width: 100%;
  height: 100%;
}

</style>
