<template lang="html">
  <div id="main-page">
    <h2>Main</h2>
    <beer-list :filteredBeers="filteredBeers"/>
    <beer-details />
  </div>
</template>

<script>
import BeerDetails from './BeerDetails.vue'
import BeerList from './BeerList.vue'

export default {
  name: 'main-page',
  props: ['filteredBeers'],
  components: {
    'beer-details': BeerDetails,
    'beer-list': BeerList
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
