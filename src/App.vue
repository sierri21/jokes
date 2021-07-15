<template>
  <div id="app" class="container">
    <search-component
      v-model="searchFilter"
      @search="searchJokes" />
    <div v-if="message">
      {{message}}
    </div>
    <div class="jokes__wrapper" v-else>
      <joke-card v-for="joke in jokes" :key="joke.id" :joke="joke"/>
    </div>
    <div class="pagination">
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
  },
  data: function () {
    return {
      jokes: {},
      searchFilter: '',
      message: ''
    }
  },
  methods: {
    async retrieveJokes () {
      const res = await fetch(' https://v2.jokeapi.dev/joke/Any?amount=10')
      let data = await res.json()
      if (!data.error) {
        data = this.getLikes(data.jokes)
        this.jokes = data
        this.message = ''
      }
    },
    async searchJokes () {
      if (!this.searchFilter) {
        this.retrieveJokes()
        return
      }
      this.jokes = {}
      const res = await fetch(`https://v2.jokeapi.dev/joke/Any?contains=${this.searchFilter}`)
      let data = await res.json()
      if (!data.error) {
        data = this.getLikes([data])
        this.jokes = data
        this.searchFilter = ''
        this.message = ''
      } else {
        this.message = data.message
      }
      // console.log(this.jokes)
    },
    getLikes (jokes) {
      const ids = localStorage.ids.split(',').map(el => parseInt(el))
      return jokes.map(el => {
        el.like = ids.includes(el.id)
        return el
      })
    }
  },
  mounted () {
    console.log(123)
    this.retrieveJokes()
  }
})
</script>

<style lang="scss">
  .container {
    max-width: 900px;
    margin: 0 auto;
    height: 100%;
    display: grid;
    grid-template-rows: 100px 1fr 80px;
  }
  .jokes__wrapper {
    // overflow-y: scroll;
    // border: 1px solid black;
    // display: flex;
    // flex-direction: column;
  }
</style>
