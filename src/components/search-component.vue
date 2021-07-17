<template>
  <div class="search">
    <form class="search__wrapper" @click.prevent="">
      <input type="text"
             :value="value"
             @input="emitValue"
             class="search__input">
      <button class="search__btn" @click="emitSearch"></button>
    </form>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'search-component',
  props: {
    value: {
      type: String,
      default: ''
    }
  },
  methods: {
    emitValue (e: { target: { value: string } }) {
      this.$emit('input', e.target.value)
    },
    emitSearch ():void {
      this.$emit('search')
    }
  }
})
</script>

<style scoped lang="scss">
  .search {
    &__wrapper {
      width: 800px;
      height: 50px;
      margin: 2rem auto;
      border: 1px solid #000;
      position: relative;
      border-radius: 12px;
      overflow: hidden;
      background-color: white;
    }
    &__input {
      position: absolute;
      top: 0;
      left: 0;
      transform: translate(-5px ,-5px);
      width: 750px;
      border: none;
      height: 54px;
      font-size: 1.5rem;
      padding-left: 15px;
      &:focus {
        outline: none;
      }
    }
    &__btn {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      right: 10px;
      height: 40px;
      width: 40px;
      border-radius: 50%;
      border: 1px solid #000;
      outline: none;
      cursor: pointer;
      background-image: url('../../src/assets/search.svg');
      background-size: cover;
      background-color: inherit;
      border: none;
    }
  }
</style>
