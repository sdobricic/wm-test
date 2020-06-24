<template>
  <div id="app">
    <h1>{{ packageInfo.title }}</h1>
    <p>{{ packageInfo.promo_text }}</p>
    <select v-model="selectedContract">
      <option disabled value="">Please select one</option>
      <option v-for="option in packageInfo.contract_length.contract_length_options" :key="option">{{ option }}</option>
    </select>
    <div class="wrapper">
      <Paket class="item" v-for="item in packageInfo.items" :key="item.id" :selected-contract="selectedContract" :package-data="item"/>
    </div>
  </div>
</template>

<script>
import Paket from './components/Paket.vue'

export default {
  name: 'App',
  components: {
    Paket
  },
  data() {
    return {
      packageInfo: {
        contract_length: {}
      },
      selectedContract: ''
    }
  },
  created () {
    this.fetchData()
  },
  methods: {
    fetchData () {
      fetch('http://www.mocky.io/v2/5ed511c53300005f00f7a790')
              .then(response => response.json())
              .then(data => {
                this.packageInfo = data;
                this.selectedContract = this.packageInfo.contract_length.preselected_contract_length;
              });
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.wrapper {
  display: flex;
  align-items: flex-start;
  justify-content: space-around;
  flex-wrap: wrap;
  .item {
    flex: .2;
    background-color: orange;
  }
}
</style>
