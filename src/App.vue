<template>
  <div id="app">
    <site-header/>
    <site-sidebar
    :allCountries="findByField('country')"
    :allCategories="findByField('cat_name')"
    :allBreweries="findByField('name_breweries')"
    :allStyles="findByField('style_name')" />
    <site-main :filteredBeers="filteredBeers"/>
  </div>
</template>

<script>
import { eventBus } from './main.js'
import SiteHeader from './components/SiteHeader.vue'
import SiteSidebar from './components/SiteSidebar.vue'
import SiteMain from './components/SiteMain.vue'

export default {
  name: 'app',
  data() {
    return {
      allBeers: [],
      filterObject: {
        country: '',
        cat_name: '',
        style_name: '',
        name_breweries: ''
      }
    }
  },
  computed: {
    filteredBeers: function() {
      return this.allBeers
      .filter(beer => {
        return (beer.fields.country === this.filterObject.country || this.filterObject.country === '')
      })
      .filter(beer => {
        return (beer.fields.cat_name === this.filterObject.cat_name || this.filterObject.cat_name === '')
      })
      .filter(beer => {
        return (beer.fields.style_name === this.filterObject.style_name ||
          this.filterObject.style_name === '')
        })
        .filter(beer => {
          return (beer.fields.name_breweries === this.filterObject.name_breweries || this.filterObject.name_breweries === '')
        })
      }
    },
    components: {
      'site-header': SiteHeader,
      'site-main': SiteMain,
      'site-sidebar': SiteSidebar
    },
    mounted() {
      this.fetchBeerDetails()
      eventBus.$on('selected-field', fieldResult => {
        this.filterObject[fieldResult[0]] = fieldResult[1]
      })
    },
    methods: {
      fetchBeerDetails: function() {
        fetch("https://public-us.opendatasoft.com/api/records/1.0/search/?dataset=open-beer-database&rows=100&facet=style_name&facet=cat_name&facet=name_breweries&facet=country")
        .then(response => response.json())
        .then(response => {
          this.allBeers = response.records.filter(beer => { return beer.fields.hasOwnProperty('coordinates')
        })
      })
    },
    findByField: function(field) {
      const allFields = this.filteredBeers.map(beer => beer.fields[field])
      const uniqueFields = [...new Set(allFields)]
      const result = uniqueFields.filter(result => result !== undefined).sort()
      return result;
    }
  }
}
</script>

<style>

  * {
    margin: 0;
    padding: 0;
  }

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: black;
    display: grid;
    grid-template-columns: 30vw auto;
    grid-template-rows: 10vh 90vh;
    grid-template-areas:
    "header header"
    "sidebar main"
  }
</style>
