<template lang="html">
  <div id="beer-list">
    <h3>BeerList</h3>
    <ul>
      <li
      v-for="(beer, index) in filteredBeers"
      :key="index"
      v-on:click="handleClick(beer)">
      {{ beer.fields.name }}
    </li>
  </ul>
</div>
</template>

<script>
import { eventBus } from '../main.js'

export default {
  name: 'beer-list',
  methods: {
    handleClick: function(beer) {
      eventBus.$emit('selected-beer', beer)
    }
  },
  props: ['filteredBeers']
}
</script>

<style lang="css" scoped>
</style>
