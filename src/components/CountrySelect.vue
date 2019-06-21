<template lang="html">
  <div>
    <select id="country-select" v-model="selectedCountry">
      <option value="">Select a country...</option>
      <option v-for="(country, index) in allCountries" :key="index" :value="country">{{ country }}</option>
    </select>
  </div>
</template>

<script>
export default {
  name: "country-select",
  props: ['allCountries'],
  data() {
    return {
      selectedCountry: null
    }
  }
}
</script>

<style lang="css" scoped>
</style>
