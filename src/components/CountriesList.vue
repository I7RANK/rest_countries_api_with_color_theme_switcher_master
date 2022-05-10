<script>
import CountryItem from "./CountryItem.vue";

export default {
  components: {
    CountryItem,
  },
  data() {
    return {};
  },
  props: {
    countries: Array,
  },
};
</script>

<template>
  <section class="countries-content">
    <ul class="flex flex-wrap justify-around">
      <CountryItem
        :key="country.name.common"
        :country="country"
        v-for="country in countries"
      />
    </ul>
  </section>
</template>

<style>
</style>
