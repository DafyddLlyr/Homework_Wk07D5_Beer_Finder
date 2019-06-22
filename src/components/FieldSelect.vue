<template lang="html">
  <div>
    <select id="field-select" v-model="selectedField" @change="handleSelect">
      <option value="">All {{ displayField }}</option>
      <option v-for="(field, index) in allFields" :key="index" :value="field">{{ field }}</option>
    </select>
  </div>
</template>

<script>
import { eventBus } from '../main.js'
export default {
  name: "field-select",
  props: ['allFields', 'field', 'displayField'],
  data() {
    return {
      selectedField: ""
    }
  },
  methods: {
    handleSelect: function() {
      eventBus.$emit('selected-field', [this.field, this.selectedField])
    }
  }
}
</script>

<style lang="css" scoped>

#field-select {
  font-size: 1rem;
  font-family: inherit;
  width: 20vw;
}

</style>
