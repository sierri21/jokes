<template>
  <div id="app" class="container">
    <search-component
      v-model="searchFilter"
      @search="searchJokes" />
    <div class="message" v-if="isFetching" >
      <img src="@/assets/loading.svg" alt="loading" class="loader">
    </div>
    <div v-if="!isFetching && message" class="message">
      {{message}}
    </div>
    <div class="jokes__wrapper" v-else-if="!isFetching && jokes.length">
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
      message: '',
      isFetching: false
    }
  },
  methods: {
    async retrieveJokes () {
      this.isFetching = true
      const res = await fetch(' https://v2.jokeapi.dev/joke/Any?amount=10')
      let data = await res.json()
      if (!data.error) {
        data = this.getLikes(data.jokes)
        this.jokes = data
        this.message = ''
      }
      this.isFetching = false
    },
    async searchJokes () {
      this.isFetching = true
      if (!this.searchFilter) {
        this.retrieveJokes()
        return
      }
      this.jokes = {}
      const res = await fetch(`https://v2.jokeapi.dev/joke/Any?contains=${this.searchFilter}&amount=10`)
      let data = await res.json()
      if (!data.error) {
        data = this.getLikes(data.jokes)
        this.jokes = data
        this.searchFilter = ''
        this.message = ''
      } else {
        this.message = data.message
      }
      this.isFetching = false
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
  .loader {
    margin-top: 60px;
    width: 50px;
    height: 50px;
    animation: 1s linear 0s normal none infinite running rot;
  }
  @keyframes rot {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }

  .message {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 1.5rem;
  }
</style>
