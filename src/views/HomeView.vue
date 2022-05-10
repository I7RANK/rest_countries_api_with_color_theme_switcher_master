<script>
import magnifyingGlassSolid from '../assets/icons/magnifying-glass-solid.svg';
import allCountries from './countries';

import CountriesList from '../components/CountriesList.vue';

let timerStopTyping;

function removeDiacritics(text) {
  return text.normalize('NFD').replace(/[\u0300-\u036f]/g,"");
}

export default {
  components: {
    CountriesList,
  },
  data() {
    return {
      magnifyingGlassSolid,
      filterByName: 'all',
      filterByRegion: 'all',
      regions: ['all', 'Africa', 'Americas', 'Asia', 'Europe', 'Oceania'],
      countries: [],
      countriesFiltered: [],
    };
  },
  watch: {
    filterByRegion(newValue) {
      if (newValue === 'all') {
        this.getAllCountries();
        return;
      }
      this.getCountriesByRegion(this.filterByRegion);
    },
    filterByName(newValue) {
      this.countriesFiltered = this.countries.filter(country => {
        let name = removeDiacritics(country.name.common).toLowerCase();

        if (name.includes(newValue.toLowerCase())) return country;
      });
    },
    countries(newValue) {
      this.$refs.inputFilterByName.value = '';
      this.countriesFiltered = newValue;
    }
  },
  methods: {
    async getAllCountries() {
      const url = 'https://restcountries.com/v3.1/all';

      try {
        const res = await fetch(url);
        this.countries = (await res.json());
      } catch (error) {
        console.log('Error! Could not reach the API. ' + error);
      }
    },
    async getCountriesByRegion(region) {
      const url = `https://restcountries.com/v3.1/region/${region}`;

      try {
        const res = await fetch(url);
        this.countries = (await res.json());
      } catch (error) {
        console.log('Error! Could not reach the API. ' + error);
      }
    },
    applyFilter(e) {
      this.filterByRegion = e.target.value;
    },
    isTyping(e) {
      clearTimeout(timerStopTyping);

      timerStopTyping = setTimeout(() => {
        this.filterByName = e.target.value;
      }, 1000);
    },
  },
  mounted() {
    this.getAllCountries();
  }
};
</script>

<template>
  <div class="p-4">
    <section class="filter-section flex flex-wrap justify-between items-center">
      <div class="search-content min-w-full sm:min-w-0 sm:max-w-2xl sm:w-2/6 mb-4 sm:mb-0 flex items-center justify-around p-3 rounded-md shadow-md">
        <img :src="magnifyingGlassSolid" alt="" class="h-4 mx-3">
        <input ref="inputFilterByName" @keyup="isTyping" class="grow p-2" type="text" placeholder="Search for a country...">
      </div>
      <div class="filter-regions-content p-3 rounded-md shadow-md flex items-center">
        <select class="cursor-pointer" name="regions" id="regions" @input="this.applyFilter">
          <option :key="region" :value="region" v-for="region in regions">{{ region }}</option>
        </select>
      </div>
    </section>

    <CountriesList :countries="countriesFiltered"/>
  </div>
</template>

<style>
.search-content, .filter-regions-content, .filter-regions-content select {
  background: var(--secondary-background);
}
</style>
