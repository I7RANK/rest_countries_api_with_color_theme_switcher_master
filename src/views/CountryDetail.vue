<script>
import { RouterLink } from "vue-router";

import arrowLeftSolid from '../assets/icons/arrow-left-solid.svg'

export default {
  data() {
    return {
      arrowLeftSolid,
      country: {},
    };
  },
  computed: {
    countryFlag() {
      console.log(this.country);
      return this.country.flags ? this.country.flags.svg : '';
    },
    countryName() {
      return this.country.name ? this.country.name.common : '';
    },
    countryNativeName() {
      if (!this.country.name) return '';

      const nativeNameKey = Object.keys(this.country.name.nativeName)[0];
      return this.country.name.nativeName[nativeNameKey].common;
    },
    countryTLD() {
      return this.country.tld ? this.country.tld.join(', ') : '';
    },
    countryLanguages() {
      if (!this.country.languages) return '';

      return Object.values(this.country.languages).join(', ');
    },
    countryCurrencies() {
      if (!this.country.currencies) return '';

      const currencies = Object.values(this.country.currencies)
      return currencies.map(obj => obj.name).join(', ');
    },
    countryBorders() {
      return this.country.borders ? this.country.borders : [];
    }
  },
  methods: {
    async getCountryByName(name) {
      const url = `https://restcountries.com/v3.1/name/${name}`;

      try {
        const res = await fetch(url);
        this.country = (await res.json())[0];
        console.log(res.status);
        console.log(this.country);
      } catch (error) {
        console.log('Error! Could not reach the API. ' + error);
      }
    },
    async getCountryByCode(code) {
      const url = `https://restcountries.com/v3.1/alpha/${code}`;

      console.log(code);

      try {
        const res = await fetch(url);
        this.country = (await res.json())[0];
        console.log(res.status);
        console.log(this.country);
      } catch (error) {
        console.log('Error! Could not reach the API. ' + error);
      }
    }
  },
  mounted() {
    this.getCountryByCode(this.$route.params.name);
  }
}
</script>

<template>
  <div class="p-4">
    <div class="mb-10 mt-4">
      <RouterLink to="/">
        <button class="
          btn-back cursor-pointer
          flex items-center
          px-4 py-1 shadow-md rounded-md
          active:shadow-sm">
          <img class="w-4 mr-4" :src="arrowLeftSolid" alt="">
          Back
        </button>
      </RouterLink>
    </div>

    <section class="p-6">
      <img class="" :src="countryFlag" alt="">

      <div>
        <h3 class="text-xl font-bold mb-2 mt-8">{{ countryName }}</h3>
        <div class="mt-4">
          <p>
            <span class="font-semibold">Native Name: </span>
            {{ countryNativeName }}
          </p>
          <p>
            <span class="font-semibold">Population: </span>
            {{ country.population }}
          </p>
          <p>
            <span class="font-semibold">Region: </span>
            {{ country.region }}
          </p>
          <p>
            <span class="font-semibold">Sub Region: </span>
            {{ country.subregion }}
          </p>
          <p>
            <span class="font-semibold">Capital: </span>
            {{ country.capital ? country.capital[0] : '' }}
          </p>
        </div>

        <div class="mt-8">
          <p>
            <span class="font-semibold">Top Level Domain: </span>
            {{ countryTLD }}
          </p>
          <p>
            <span class="font-semibold">Currencies: </span>
            {{ countryCurrencies }}
          </p>
          <p>
            <span class="font-semibold">Languages: </span>
            {{ countryLanguages }}
          </p>
        </div>

        <div class="mt-8">
          <span class="font-semibold">Border Countries: </span>
          <div class="flex justify-start flex-wrap mt-4">
            <RouterLink :to="`/${countryBorder}`" :key="countryBorder"
                v-for="countryBorder in countryBorders" @click="this.getCountryByCode(countryBorder)">
              <button class="px-2 mx-1 my-2 rounded-md shadow-md">
                {{ countryBorder }}
              </button>
            </RouterLink>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style>
.btn-back {
  background: var(--secondary-background);
}
</style>
