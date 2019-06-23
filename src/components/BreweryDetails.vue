<template lang="html">
  <div class="brewery-details">
    <h3>{{ beer.name_breweries }}</h3>
    <h4><span v-if='beer.state'>{{ beer.state }}, </span> {{ beer.country }} </h4>
  </div>
</template>

<script>
export default {
  name: 'brewery-details',
  props: ['beer'],
  data() {
    return {
      breweryBeers: null
    }
  },
  computed: {
    cleanName: function() {
      return this.beer.name_breweries.split(" ").join("+")
    },
    // beerCount: function(),
    // beerStyles: function(),
    // beerCateogries: function(),
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
  width: 100%;
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

</style>
