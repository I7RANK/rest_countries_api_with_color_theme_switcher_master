<script>
export default {
  props: {
    title: String
  }
}
</script>

<template>
  <div class="about">
    <h1>This is an about page, name: {{ $route.params.name }}, prop: {{ title }}</h1>
  </div>
</template>

<style>
</style>
