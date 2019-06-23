<template lang="html">
  <div class="brewery-details">
    <h3>{{ beer.name_breweries }}</h3>
    <h4><span v-if='beer.state'>{{ beer.state }}, </span> {{ beer.country }} </h4>
    <a :href="beer.website" class="website" v-if="beer.website">{{ beer.website }}</a>
    <br>
    <br>
    <p>Our database has <b>{{ this.beerCount }}</b>
      beer<span v-if="this.beerCount > 1">s</span>
      from {{ beer.name_breweries }}, across <b>{{ this.beerStylesCount }}</b>
      style<span v-if="this.beerStylesCount > 1">s</span>.
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
  name: 'brewery-details',
  props: ['beer'],
  data() {
    return {
      breweryBeers: [],
      styleChartOptions: {
        chartArea: {
          width: '90%',
          height: '90%'
        },
        fontName: 'Avenir',
        pieHole: 0.3,
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
      return this.beer.name_breweries.split(" ").join("+");
    },
    beerCount: function() {
      return this.breweryBeers.length;
    },
    beerStyles: function() {
      let uniqueStyles = [...new Set(this.breweryBeers.map(beer => beer.style_name))]
      return uniqueStyles.filter(style => style !== undefined);
    },
    beerStylesCount: function() {
      return this.beerStyles.length;
    },
    beerStyleFrequency: function() {
      let emptyStyleArray = this.beerStyles.map(style => [style, 0] )
      let result = emptyStyleArray.map(style => { this.breweryBeers
        .forEach(beer => { if (beer.style_name === style[0]) {
          style[1] +=1
        }})
        return [style[0], style[1]]
      })
      result.unshift(['Style', 'Count'])
      return result;
    },
    beerCateogries: function() {
      let uniqueCategories = [...new Set(this.breweryBeers.map(beer => beer.cat_name))]
      return uniqueCategories.filter(category => category !== undefined);
    },
    beerABVs: function() {
      let allABVs = this.breweryBeers.map(beer => beer.abv);
      return allABVs.filter(abv => abv !== 0)
    }
  },
  mounted() {
    fetch(`https://public-us.opendatasoft.com/api/records/1.0/search/?dataset=open-beer-database&rows=56&refine.name_breweries=${this.cleanName}`)
    .then(response => response.json())
    .then(result => this.breweryBeers = result.records.map(beer => beer.fields))
  }
}
</script>

<style lang="css" scoped>

.brewery-details {
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

.logo {
  margin-top: -1vw;
}

h5 {
  margin-bottom: 0vw;
}

.chart {
  margin-bottom: 2vw;
}

</style>
