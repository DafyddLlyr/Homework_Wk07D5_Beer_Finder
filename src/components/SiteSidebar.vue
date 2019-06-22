<template lang="html">
  <div id="sidebar">
    <h2>Sidebar</h2>

    <div class="select-container">
      <h3>Country</h3>
      <field-select
        :allFields="allCountries"
        field="country"
        displayField="countries"/>
    </div>

    <div class="select-container" v-if='allCategories.length > 0'>
      <h3>Category</h3>
      <field-select
        :allFields="allCategories"
        field="cat_name"
        displayField="categories"/>
    </div>

    <div class="select-container" v-if='allStyles.length > 0'>
      <h3>Style</h3>
      <field-select
        :allFields="allStyles"
        field="style_name"
        displayField="styles"/>
    </div>

    <div class="select-container">
      <h3>Brewery</h3>
      <field-select
        :allFields="allBreweries"
        field="name_breweries"
        displayField="breweries"/>
    </div>


  </div>
</template>

<script>
import FieldSelect from './FieldSelect.vue'

export default {
  name: 'sidebar',
  props: ['allCountries', 'allStyles', 'allCategories', 'allBreweries'],
  components: {
    'field-select': FieldSelect
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

.select-container {
  margin: 1vw 0;
}

</style>
