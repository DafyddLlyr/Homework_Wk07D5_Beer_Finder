<template lang="html">
  <div id="beer-list">
    <p>BeerList</p>
    <ul>
      <li v-for="(beer, index) in filteredBeers" :key="index">{{ beer.fields.name }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'beer-list',
  props: ['filteredBeers']
}
</script>

<style lang="css" scoped>
</style>
