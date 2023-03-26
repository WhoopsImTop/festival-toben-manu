<template>
  <div>
    <header-component></header-component>
    <nuxt></nuxt>
    <footer-component></footer-component>
  </div>
</template>

<script>
import headerComponent from '../components/headerComponent.vue';
export default {
  components: { headerComponent },
};
</script>

<style>
</style>