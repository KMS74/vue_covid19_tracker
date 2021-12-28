<template>
  <select
    @change="onChange"
    v-model="selected"
    class="form-select my-10 block w-full border p-3 rounded"
  >
    <option value="0">Select Country</option>
    <option v-for="country in countries" :value="country.ID" :key="country.ID">
      {{ country.Country }}
    </option>
  </select>
</template>

<script>
export default {
  name: "CountrySelect",
  data() {
    return {
      selected: 0,
    };
  },
  props: ["countries"],
  methods: {
    onChange() {
      const country = this.countries.find((item) => item.ID === this.selected);
      console.log(country);
      // emit cutom event(pass data from child to parnet)
      this.$emit("get_country", country);
    },
  },
};
</script>
