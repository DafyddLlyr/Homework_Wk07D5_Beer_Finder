<template lang="html">
  <div class="country-details">
    <h3 class="country-header">{{ beer.country }}</h3>
    <img :src="flagURL" class="flagImage"/>
    <p>Our database has <b>{{ this.beerCount }}</b>
      beer<span v-if="this.beerCount > 1">s</span>
      from {{ beer.country }}, across <b>{{ this.beerStylesCount }}</b>
      style<span v-if="this.beerStylesCount > 1">s</span>. {{ beer.country }} has <b>{{ this.countryBreweryCount }}</b> unique breweries on record.
    </p>
    <br>
    <h5>Style Breakdown</h5>
    <GChart
    class="chart"
    type="PieChart"
    :data="beerStyleFrequency"
    :options="styleChartOptions"
    />
  </div>
</template>

<script>
export default {
  name: 'country-details',
  data() {
    return {
      countryBeers: '',
      flagURL: null,
      styleChartOptions: {
        chartArea: {
          width: '90%',
          height: '90%'
        },
        fontName: 'Avenir',
        pieHole: 0.3,
        sliceVisibilityThreshold: 0.02,
        legend: {
          alignment: 'center',
          textStyle: {
            bold: true
          }
        }
      }
    }
  },
  props: ['beer'],
  computed: {
    beerCount: function() {
      if (this.countryBeers) {
        return this.countryBeers.length;
      }
    },
    cleanName: function() {
      return this.beer.country.split(" ").join("+");
    },
    beerStyles: function() {
      if (this.countryBeers) {
        let uniqueStyles = [...new Set(this.countryBeers.map(beer => beer.style_name))]
        return uniqueStyles.filter(style => style !== undefined);
      }
    },
    beerStylesCount: function() {
      if (this.countryBeers) {
        return this.beerStyles.length;
      }
    },
    countryBreweries: function() {
      if (this.countryBeers) {
        let uniqueBreweries = [...new Set(this.countryBeers.map(beer => beer.name_breweries))]
        return uniqueBreweries.filter(brewery => brewery !== undefined);
      }
    },
    countryBreweryCount: function() {
      if (this.countryBeers) {
        return this.countryBreweries.length;
      }
    },
    beerStyleFrequency: function() {
      if (this.countryBeers) {
        let emptyStyleArray = this.beerStyles.map(style => [style, 0] )
        let result = emptyStyleArray.map(style => { this.countryBeers
          .forEach(beer => { if (beer.style_name === style[0]) {
            style[1] +=1
          }})
          return [style[0], style[1]]
        })
        result.unshift(['Style', 'Count'])
        return result;
      }
    }
  },
  mounted() {
    fetch(`https://public-us.opendatasoft.com/api/records/1.0/search/?dataset=open-beer-database&rows=5000&facet=style_name&facet=cat_name&facet=name_breweries&facet=country&refine.country=${this.cleanName}`)
    .then(response => response.json())
    .then(result => this.countryBeers = result.records.map(beer => beer.fields))
    fetch(`https://restcountries.eu/rest/v2/name/${this.beer.country}`)
    .then(response => response.json())
    .then(result => this.flagURL = result[0].flag)
  }
}
</script>

<style lang="css" scoped>

.country-details {
  width: 90%;
  height: 100%;
  margin-top: -3vw;
}

h3 {
  cursor: default;
  text-align: center;
  margin: 1vw;
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

.flagImage {
  width: 20vw;
}

</style>
