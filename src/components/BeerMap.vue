<template lang="html">
  <div id="beer-map">
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
        let beerDetails =
        `<div class='beer-details'>
        <h3 class="beer-header">${beer.fields.name}</h3>
        <table>
          <tr>
            <th>Style: </th>
            <td>${beer.fields.style_name}</td>
          </tr>
          <tr>
            <th>Category: </th>
            <td>${beer.fields.cat_name}</td>
          </tr>
          <tr>
            <th>ABV: </th>
            <td>${beer.fields.abv.toFixed(2)}%</td>
          </tr>
          <tr>
            <th>Brewery: </th>
            <td>${beer.fields.name_breweries.trim()}</td>
          </tr>
          <tr>
            <th>Location: </th>
            <td>${beer.fields.state}, ${beer.fields.country}</td>
          </tr>
        </table>
        `
        return L.marker(beer.fields.coordinates)
          .bindPopup(beerDetails).openPopup();
      })
      this.markers = L.featureGroup(markerArray)
      this.beerMap.addLayer(this.markers);
      this.beerMap.flyToBounds(this.markers.getBounds());
    }
  },
  mounted() {
    this.beerMap = L.map('beer-map').setView([55.8634087,-4.2567716], 4);
    L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}', {
      attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
      maxZoom: 18,
      id: 'mapbox.streets',
      accessToken: keys['leaflet']
    }).addTo(this.beerMap);
  }
}
</script>

<style lang="css">

#beer-map, .container {
  width: 100%;
  height: 100%;
}

.beer-details .leaflet-popup-content-wrapper {
  width: auto;
}

th {
  padding-right: 1vw;
}

table {
  cursor: default;
}

.beer-header {
  cursor: default;
  text-align: center;
  margin: 1vw;
  font-size: 1.1rem;
}

</style>
