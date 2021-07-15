<template>
  <div id="app">
    <search-component />
    <div class="jokes__wrapper">
      <joke-card />
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import searchComponent from './components/search-component.vue'
import jokeCard from '@/components/joke-card.vue'
export default Vue.extend({
  name: 'App',
  components: {
    searchComponent,
    jokeCard
  }
})
</script>

<style lang="scss">

</style>
