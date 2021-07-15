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
                like
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
  methods: {
    likeJoke () {
      if (!this.joke.like) {
        if (!localStorage.ids) {
          localStorage.ids = this.joke.id
        } else {
          localStorage.ids += ',' + this.joke.id
        }
        this.joke.like = true
      } else {
        this.joke.like = false
        const ids = localStorage.ids.split(',')
        ids.splice(ids.findIndex(el => parseInt(el) === this.joke.id), 1)
        localStorage.ids = ids.join('')
      }
    }

  }
})
</script>

<style scoped lang="scss">
    .card {
        max-width: 750px;
        border: 1px solid gray;
        min-height: 20px;
        margin: 1rem auto;
        display: grid;
        grid-template-columns: 1fr 50px;
        padding: 10px;
        border-radius: 12px;
        align-items: center;
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
            }
        }
        &.active {
            background-color: rgba(159, 255, 64, 0.4);
        }
    }
</style>
