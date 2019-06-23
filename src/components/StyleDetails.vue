<template lang="html">
  <div class="style-details">
    <h3>{{ beer.style_name }}</h3>
    <br>
    <p>Our database has <b>{{ this.beerCount }}</b>
      beer<span v-if="this.beerCount > 1">s</span> of this style across <b>{{ this.beerCountryCount }}</b>
      countr<span v-if="this.beerCountryCount > 1">ies</span>
      <span v-if="this.beerCountryCount <= 1">y</span>.
      <br>
      <br>
      The average ABV for this style is <b>{{ this.averageABV }}%</b>.
    </p>
    <br>
    <h5>Style Popularity</h5>
    <GChart
      class="chart"
      type="PieChart"
      :data="styleCountryFrequency"
      :options="styleChartOptions"
    />
  </div>
</template>

<script>
export default {
  name: 'style-details',
  props: ['beer'],
  data() {
    return {
      styleBeers: [],
      facets: null,
      styleChartOptions: {
        chartArea: {
          width: '90%',
          height: '90%'
        },
        fontName: 'Avenir',
        pieHole: 0.3,
        sliceVisibilityThreshold: 0.015,
        legend: {
          alignment: 'center',
          textStyle: {
            bold: true
          }
        }
      }
    }
  },
  computed: {
    cleanName: function() {
      return this.beer.style_name.split(" ").join("+");
    },
    beerCount: function() {
      return this.styleBeers.length;
    },
    beerCountries: function() {
      let uniqueCountries = [...new Set(this.styleBeers.map(beer => beer.country))]
      return uniqueCountries.filter(country => country !== undefined);
    },
    beerCountryCount: function() {
      return this.beerCountries.length;
    },
    styleCountryFrequency: function() {
      this.facets.unshift(['Country', 'Count'])
      return this.facets;
    },
    beerABVs: function() {
      let allABVs = this.styleBeers.map(beer => beer.abv);
      return allABVs.filter(abv => abv !== 0)
    },
    averageABV: function() {
      let average = this.beerABVs.reduce((a, b) => a + b) / this.beerABVs.length
      return average.toFixed(2)
    }
  },
  mounted() {
    fetch(`https://public-us.opendatasoft.com/api/records/1.0/search/?dataset=open-beer-database&rows=527&facet=country&refine.style_name=${this.cleanName}`)
    .then(response => response.json())
    .then(result => {
      this.styleBeers = result.records.map(beer => beer.fields);
      this.facets = result.facet_groups[0].facets.map(facet => [facet.name, facet.count] )
    })
  }
}
</script>

<style lang="css" scoped>

.style-details {
  width: 90%;
  height: 100%;
  margin-top: -3vw;
}

h3 {
  cursor: default;
  text-align: center;
  margin: 1vw;
  margin-bottom: 0;
  font-size: 2rem;
  font-family: 'Lobster', cursive;
  color: #F06543;
}

h5 {
  margin-bottom: 0vw;
}

.chart {
  margin-bottom: 2vw;
}

</style>
