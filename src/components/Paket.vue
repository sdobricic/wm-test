<template>
  <div>
    <h1>{{ packageData.name }}</h1>
    <div v-if="packageData.is_featured">Featured</div>
    <h3>Cene:</h3>
    <div v-if="priceRecurring" class="recurring">Old Price Recurring: {{ priceRecurring }}</div>
    <div>Price Recurring: {{ price }}</div>
    <div v-if="priceRecurring" v-html="packageData.prices.old_price_promo_text"></div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    packageData: Object,
    selectedContract: String
  },
  computed: {
    priceRecurring() {
      return this.packageData.prices.old_price_recurring[this.selectedContract];
    },
    price() {
      return this.packageData.prices.price_recurring[this.selectedContract];
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.recurring {
  text-decoration: line-through;
}
</style>
