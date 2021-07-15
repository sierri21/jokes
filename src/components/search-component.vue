<template>
  <div class="search">
    <div class="search__wrapper">
      <input type="text" class="search__input">
      <button class="search__btn"></button>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'search-component',
  props: {
  }
})
</script>

<style scoped lang="scss">
  .search {
    &__wrapper {
      width: 800px;
      height: 30px;
      margin: 2rem auto;
      border: 1px solid #000;
      position: relative;
      border-radius: 12px;
      overflow: hidden;
    }
    &__input {
      position: absolute;
      top: 0;
      left: 0;
      transform: translate(-5px ,-5px);
      width: 750px;
      border: none;
      height: 34px;
      padding-left: 15px;
    }
    &__btn {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      right: 10px;
      height: 20px;
      width: 20px;
      border-radius: 50%;
      border: 1px solid #000;
      outline: none;
      cursor: pointer;
    }
  }
</style>
