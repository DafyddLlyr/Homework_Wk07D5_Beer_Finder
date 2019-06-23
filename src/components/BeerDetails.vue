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
      <tr>
        <th>ABV: </th>
        <td>{{ beer.fields.abv.toFixed(2) }}%</td>
      </tr>
      <tr>
        <th>Brewery: </th>
        <td>{{ beer.fields.name_breweries.trim() }}</td>
      </tr>
      <tr>
        <th>Location: </th>
        <td>
          <span v-if="beer.fields.state != undefined">{{ beer.fields.state }},  </span>{{ beer.fields.country }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
import { eventBus } from '../main.js'

export default {
  name: 'beer-details',
  props: ['beer']
}
</script>

<style lang="css" scoped>

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
