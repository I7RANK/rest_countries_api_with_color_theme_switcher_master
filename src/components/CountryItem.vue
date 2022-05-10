<template>
  <li class="rounded-md shadow-md my-10 sm:mx-8 sm:max-w-xs">
    <img class="rounded-t-md max-w-full" :src="uy" alt="Country flag">
    <div class="p-6">
      <h3 class="text-xl font-bold mb-2">{{ country.name.common }}</h3>
      <p>
        <span class="font-semibold">Population: </span>{{ country.population }}
      </p>
      <p>
        <span class="font-semibold">Region: </span>{{ country.region }}
      </p>
      <p>
        <span class="font-semibold">Capital: </span>{{ country.capital ? country.capital[0] : '' }}
      </p>
    </div>
  </li>
</template>

<script>
import uy from './uy.svg';

export default {
  data() {
    return {
      uy,
    };
  },
  props: {
    country: Object,
  }
}
</script>

<style>
li {
  background: var(--primary-background);
}
</style>