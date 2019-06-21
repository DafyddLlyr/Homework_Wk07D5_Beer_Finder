<template>
  <div id="app">
    <site-header/>
    <site-sidebar :allCountries="allCountries" />
    <site-main :allBeers="allBeers"/>
  </div>
</template>

<script>
import SiteHeader from './components/SiteHeader.vue'
import SiteSidebar from './components/SiteSidebar.vue'
import SiteMain from './components/SiteMain.vue'

export default {
  name: 'app',
  data() {
    return {
      allBeers: null
    }
  },
  computed: {
    allCountries: function() {
      const allCountries = this.allBeers.map(beer => beer.fields.country)
      const uniqueCountries = [...new Set(allCountries)]
      return uniqueCountries.sort()
    }
  },
  components: {
    'site-header': SiteHeader,
    'site-main': SiteMain,
    'site-sidebar': SiteSidebar
  },
  mounted() {
    this.fetchBeerDetails()
  },
  methods: {
    fetchBeerDetails: function() {
      fetch("https://public-us.opendatasoft.com/api/records/1.0/search/?dataset=open-beer-database&rows=100&facet=style_name&facet=cat_name&facet=name_breweries&facet=country")
      .then(response => response.json())
      .then(response => {
        console.log(response.records)
        this.allBeers = response.records
      })
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
  grid-template-columns: 20vw auto;
  grid-template-rows: 10vh 90vh;
  grid-template-areas:
  "header header"
  "sidebar main"
}
</style>
