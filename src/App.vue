<template>
  <div id="app">
    <div class="main-heading">
      <h1>{{ packageInfo.title }}</h1>
      <select v-model="selectedContract">
        <option disabled value="">Please select one</option>
        <option
          v-for="option in packageInfo.contract_length.contract_length_options"
          :key="option"
          >{{ option }}</option
        >
      </select>
    </div>

    <div class="wrapper">
      <Paket
        class="item"
        v-for="item in packageInfo.items"
        :key="item.id"
        :selected-contract="selectedContract"
        :package-data="item"
        :assets="packageInfo.assets"
      />
    </div>
  </div>
</template>

<script>
import Paket from "./components/Paket.vue";

export default {
  name: "App",
  components: {
    Paket,
  },
  data() {
    return {
      packageInfo: {
        contract_length: {},
        assets: {},
      },
      selectedContract: "",
    };
  },
  created() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      fetch("http://www.mocky.io/v2/5ed511c53300005f00f7a790")
        .then((response) => response.json())
        .then((data) => {
          this.packageInfo = data;
          this.selectedContract = this.packageInfo.contract_length.preselected_contract_length;
        });
    },
  },
};
</script>

<style>
@import "./assets/fonts/fonts.css";
</style>

<style lang="scss">
#app {
  font-family: "Calibri", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-bottom: 60px;
}

.main-heading {
  display: block;
  margin-bottom: 120px;
}

* {
  box-sizing: border-box;
}

h1 {
  font-family: "Calibri";
  font-weight: bold;
  font-style: italic;
  font-size: 48px;
  line-height: 48px;
  margin-top: 0;
  margin-bottom: 0;
  color: #742d6c;
}

p {
  margin-top: 0;
  margin-bottom: 0;
}

.wrapper {
  display: flex;
  flex-wrap: wrap;
  margin: 0 auto;
  justify-content: center;
  .item {
    display: flex;
    width: 340px;
    margin-right: 30px;
    max-width: 340px;
  }
}
</style>
