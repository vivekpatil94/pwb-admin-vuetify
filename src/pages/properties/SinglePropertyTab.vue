<template>
  <!-- basic -->
  <keep-alive>
    <component :is="propertyDetailsComponent" v-bind:currentProperty="currentProperty"></component>
  </keep-alive>
</template>
<script>
import PropertyPhotosDetails from '@/components/properties/PropertyPhotosDetails'
import PropertyLocationDetails from '@/components/properties/PropertyLocationDetails'
import PropertyGeneralDetails from '@/components/properties/PropertyGeneralDetails'
import PropertyTextDetails from '@/components/properties/PropertyTextDetails'
import PropertyFeaturesDetails from '@/components/properties/PropertyFeaturesDetails'
import PropertySaleDetails from '@/components/properties/PropertySaleDetails'
export default {
  data() {
    return {}
  },
  components: {
    PropertyGeneralDetails,
    PropertyTextDetails,
    PropertyFeaturesDetails,
    PropertySaleDetails,
    PropertyLocationDetails,
    PropertyPhotosDetails
  },
  computed: {
    propertyDetailsComponent() {
      let currentTabName = this.$route.params["tabName"]
      // TODO: - use lodash for below:
      let currentPropertyTab = this.findBy(this.$store.state.navigationStore.propertyTabs, currentTabName, 'tabValue')
      return currentPropertyTab[0].componentName
    },
    currentProperty() {
      return this.$store.state.propertiesStore.currentProperty
    }
  },
  watch: {
    '$route' (to, from) {
      this.$store.dispatch('loadPropertyFieldOptions', to.params.tabName)
    }
  },
  mounted: function() {
    // let fieldNames = "extras"
    // this.$store.dispatch('loadPropertyFieldOptions', fieldNames)
    this.$store.dispatch('loadPropertyFieldOptions', this.$route.params["tabName"])
  },
  methods: {
    findBy(list, value, column) {
      return list.filter(function(item) {
        return item[column].includes(value)
      })
    },
    orderBy(list, order, column) {
      return list.sort(function(a, b) {
        return order * (a[column] - b[column])
      })
    }
  },
}

</script>
