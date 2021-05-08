<template>
  <main v-if="!loading">
    <data-title :text="title" :dataDate="dataDate" />
    <data-boxes :stats="stats" />
    <country-select @get_country="getCountryData" :countries="countries" />
    <button
      v-if="stats.Country"
      @click.prevent="clearCountry"
      class="bg-green-700 text-white rounded p-3 mb-10 focus:outlie-none hover:bg-green-600"
    >
      Clear Country
    </button>
  </main>
  <main
    class="flex flex-col align-center justify-center text-center"
    v-if="loading"
  >
    <div class="text-gray-500 text-3xl mt-10 mb-6">Loading Data</div>
    <img class="w-24 m-auto" :src="loadingImage" alt="loading_img" />
  </main>
</template>

<script>
// @ is an alias to /src
import DataTitle from "../components/DataTitle.vue";
import DataBoxes from "../components/DataBoxes.vue";
import CountrySelect from "../components/CountrySelect.vue";

export default {
  name: "Home",
  data() {
    return {
      loading: true,
      title: "World Wide",
      dataDate: "",
      stats: {},
      countries: [],
      loadingImage: require("@/assets/Spinner-1s-240px.svg"),
    };
  },
  methods: {
    async fetchCovidData() {
      const response = await fetch("https://api.covid19api.com/summary");
      const data = response.json();
      return data;
    },
    getCountryData(country) {
      this.stats = country;
      this.title = country.Country;
    },
    async clearCountry() {
      this.loading = true;
      const data = await this.fetchCovidData();
      this.title = "World Wide";
      this.stats = data.Global;
      this.loading = false;
    },
  },
  async created() {
    const data = await this.fetchCovidData();
    console.log(data);
    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.loading = false;
  },
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
  },
};
</script>
