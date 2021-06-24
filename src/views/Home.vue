<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats"/>
    <country-selecet :countries="countries" @get-country="getCountryData" />
    <button 
    @click="clearCountryData"
    v-if="stats.Country" 
    class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">
      Clear countries
    </button>
  </main>
  <main v-else class="felx flex-col align-center justify-center text-center">
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      fetching data
    </div>
    <img :src="loadingImage" alt="" class="w-24 m-auto" />
  </main>
</template>

<script>

import DataTitle from '../components/DataTitle.vue'
import DataBoxes from '../components/DataBoxes.vue'
import CountrySelecet from '../components/CountrySelect.vue'

export default {
  name: "Home",
  components: {
    DataTitle,
    DataBoxes,
    CountrySelecet
  },
  data() {
    return {
      loading: true,
      title: "Global",
      dataDate: "",
      stats: {},
      countries: [],
      loadingImage: require("../assets/hourglass.gif"),
    };
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch("https://api.covid19api.com/summary");

      const data = res.json();

      return data;
    },
    getCountryData(country){

      this.stats=country
      this.title=country.country

    },
    async clearCountryData(){

        this.loading=true
        const data = await this.fetchCovidData()
        this.title= 'Global'
        this.stats=data.Global
        this.loading=false

    }
  },
  async created() {
    const data = await this.fetchCovidData();

    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.loading = false;
  },
};
</script>
