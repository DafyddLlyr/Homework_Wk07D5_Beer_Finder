<template lang="html">
  <div id="sidebar">
    <h2>Sidebar</h2>
    <country-select :allCountries="allCountries"/>
  </div>
</template>

<script>
import CountrySelect from './CountrySelect.vue'

export default {
  name: 'sidebar',
  props: ['allCountries'],
  components: {
    'country-select': CountrySelect
  }
}
</script>

<style lang="css" scoped>

#sidebar {
  display: flex;
  flex-direction: column;
  background-color: grey;
  grid-area: sidebar;
}

</style>
