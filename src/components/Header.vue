<template>
  <div class="header">
    <h1>{{ title }}</h1>
    <h3>for the Last {{ period }} Day{{ period > 1 ? 's' : '' }}</h3>
  </div>
</template>

<script>
export default {
  name: 'Header',
  props: {
    period: Number,
    title: String
  }
}
</script>

<style>
.header {
  text-align: center;
}
</style>