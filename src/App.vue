<template>
  <div id="app" class="container">
    <header class="header">
      <search-component
        v-model="searchFilter"
        @search="searchJokes"
      />
    </header>
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

interface IJoke {
  categoty: string,
  type: string,
  joke: string,
  flags: {
    nsfw: boolean,
    religious: boolean,
    political: boolean,
    racist: boolean,
    sexist: boolean,
    explicit: boolean
  },
  id: number,
  safe: boolean,
  lang: string,
  like: boolean
}
export default Vue.extend({
  name: 'App',
  components: {
    searchComponent,
    jokeCard
  },
  data: function () {
    return {
      jokes: [],
      searchFilter: '',
      message: '',
      isFetching: false,
      url: 'https://v2.jokeapi.dev/joke/Any?amount=10'
    }
  },
  methods: {
    async retrieveJokes () {
      this.isFetching = true
      this.jokes = []
      const res = await fetch(this.url)
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
      this.jokes = []
      if (!this.searchFilter) {
        this.retrieveJokes()
        return
      }
      const res = await fetch(this.url + `&contains=${this.searchFilter}`)
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
    },
    getLikes (jokes: IJoke[]) {
      const ids = JSON.parse(localStorage.ids)
      return jokes.map(el => {
        el.like = ids.includes(el.id)
        return el
      })
    }
  },
  mounted () {
    if (!localStorage.ids) {
      localStorage.setItem('ids', JSON.stringify([]))
    }
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
    box-shadow: 0 0 15px 0 rgba(0, 0, 0, .30);
    border-radius: 20px;
    grid-template-rows: 110px 1fr 80px;
    overflow: hidden;
  }
  .header {
    background-color: #faf1e6;
    // box-shadow: 0 0 15px 0 rgba(0, 0, 0, .30);
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
    margin-top: 3rem;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 1.5rem;
    font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
  }
</style>
