<template>
    <div class="card" :class="{'active': !!joke.like}">
        <div class="card__text" v-if="joke.joke">
            {{joke.joke}}
        </div>
        <div v-else>
            {{joke.setup}} - {{joke.delivery}}
        </div>
        <div class="card__like">
            <button @click="likeJoke">
                <svg
                    height="40"
                    viewBox="-3 -3 25 25"
                    width="40"
                    :fill="fill"
                    :stroke="stroke"
                    xmlns="http://www.w3.org/2000/svg">
                        <path d="m0 1v8c0 .552246.447693 1 1 1h3v-10h-3c-.552307 0-1 .447693-1 1z" transform="translate(0 5)"/>
                        <path d="m9.15332 5.02979h-2.9541c-.258301 0-.387695-.172363-.431152-.246582-.043457-.0737305-.131348-.270508-.0063477-.496094l1.0415-1.87549c.228516-.410645.251953-.893555.0649414-1.32471-.187012-.43164-.556152-.744629-1.0127-.858398l-.734375-.183594c-.178711-.0449219-.368164.0122071-.492676.150391l-3.9873 4.42969c-.413574.460449-.641113 1.0542-.641113 1.67236v5.23242c0 1.37842 1.12158 2.5 2.5 2.5l4.97412-.0004883c1.12305 0 2.11475-.756348 2.41113-1.83887l1.06738-4.89844c.03125-.13623.0473633-.275879.0473633-.415527 0-1.01807-.828613-1.84668-1.84668-1.84668z" transform="translate(5 .97)"/></svg>
            </button>
        </div>
    </div>
</template>

<script>
import Vue from 'vue'
export default Vue.extend({
  name: 'joke-card',
  props: {
    joke: { type: Object }
  },
  computed: {
    fill () {
      return this.joke.like ? '#064420' : '#e4efe7'
    },
    stroke () {
      return !this.joke.like ? '#064420' : '#e4efe7'
    }
  },
  methods: {
    likeJoke () {
      if (!this.joke.like) {
        const ids = JSON.parse(localStorage.ids)
        ids.push(this.joke.id)
        localStorage.ids = JSON.stringify(ids)
        this.joke.like = true
      } else {
        this.joke.like = false
        const ids = JSON.parse(localStorage.ids)
        ids.splice(ids.findIndex(el => parseInt(el) === this.joke.id), 1)
        localStorage.ids = JSON.stringify(ids)
      }
    }

  }
})
</script>

<style scoped lang="scss">
    .card {
        max-width: 750px;
        box-shadow: 0 0 5px 0 rgba(0, 0, 0, .30);
        min-height: 20px;
        margin: 1.5rem auto;
        display: grid;
        grid-template-columns: 1fr 50px;
        padding: 10px;
        border-radius: 12px;
        align-items: center;
        background-color: #fdfaf6;
        font-size: 1.5rem;
        font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
        &__like {
            margin:0 1rem;
            // border: 1px solid green;
            width: 40px;
            height: 40px;
            & button {
                width: 100%;
                height: 100%;
                cursor: pointer;
                outline: none;
                background: none;
                border: none;
                padding: 0;
                & svg {
                    margin: 0;
                    padding: 0;
                }
            }
        }
        &.active {
            background-color: #e4efe7;
        }
    }
</style>
