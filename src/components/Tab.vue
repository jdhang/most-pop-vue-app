<template>
  <li class="nav-item">
    <a v-bind:class="aClass" href="#" v-on:click="selectPeriod">{{ tab.text }}</a>
  </li>
</template>

<script>
export default {
  name: 'Tab',
  props: {
    tab: Object,
    select: Function,
    selectedPeriod: Number
  },
  computed: {
    aClass() {
      return this.tab.period === this.selectedPeriod ? 'nav-link active' : 'nav-link'
    }
  },
  methods: {
    selectPeriod: function() {
      this.select(this.tab.period)
    }
  }
}
</script>