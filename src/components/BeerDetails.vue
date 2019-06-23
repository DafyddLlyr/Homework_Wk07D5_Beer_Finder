<template lang="html">
  <div id="beer-details">
    <h3 class="beer-header"> {{ beer.fields.name }}</h3>
    <table>
      <tr>
        <th>Style: </th>
        <td>{{ beer.fields.style_name }}</td>
      </tr>
      <tr>
        <th>Category: </th>
        <td>{{ beer.fields.cat_name }}</td>
      </tr>
      <tr v-if="beer.fields.abv !== 0">
        <th>ABV: </th>
        <td>{{ beer.fields.abv.toFixed(2) }}%</td>
      </tr>
      <tr>
        <th>Brewery: </th>
        <td class="details-link" v-on:click="brewerySelect">{{ beer.fields.name_breweries.trim() }}</td>
      </tr>
      <tr>
        <th>Location: </th>
        <td>
          <span v-if="beer.fields.state != undefined">{{ beer.fields.state }},  </span><span class="details-link" v-on:click="countrySelect">{{ beer.fields.country }}</span></td>
      </tr>
    </table>
    <h4 v-if="beer.fields.hasOwnProperty('descript')" class="description-header"><b>Description</b></h4>
    <p v-if="beer.fields.hasOwnProperty('descript')" class="beer-description">{{ beer.fields.descript }}</p>
  </div>
</template>

<script>
import { eventBus } from '../main.js'

export default {
  name: 'beer-details',
  props: ['beer'],
  methods: {
    brewerySelect: function() {
      eventBus.$emit('selected-brewery', this.beer)
    },
    countrySelect: function() {
      eventBus.$emit('selected-country', this.beer)
    }
  }
}
</script>

<style lang="css" scoped>

.beer-details .leaflet-popup-content-wrapper {
  width: auto;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
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
  font-size: 1.7rem;
  font-family: 'Lobster', cursive;
  color: #F06543;
}

.details-link {
  cursor: pointer;
  text-decoration: underline;
}

.details-link:hover {
  color: #F06543;
}

.beer-description {
  margin-top: 0;
}

.description-header {
  margin-top: 2vw;
}

</style>
