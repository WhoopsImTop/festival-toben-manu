<template>
  <div class="navigation-container">
    <div class="content-container navigation-content">
      <nuxt-link to="/"><logo class="logo" /></nuxt-link>
      <div class="link-container" :class="active ? 'active' : ''" @click="closeMenu()">
        <nuxt-link class="link" to="tickets-kaufen" 
          >Tickets Kaufen</nuxt-link
        >
        <nuxt-link class="link" to="sponsor-werden" >Sponsor werden</nuxt-link
        >
      </div>
      <div
        class="burger"
        :class="active ? 'active' : ''"
        @click="active = !active"
      >
        <div class="burger-line"></div>
        <div class="burger-line"></div>
        <div class="burger-line"></div>
      </div>
    </div>
  </div>
</template>

<script>
import logo from "./logo.vue";
export default {
  components: { logo },
  data() {
    return {
      active: false,
    };
  },
  methods: {
    closeMenu() {
      document.querySelector(".burger").classList.remove("active");
      document.querySelector(".link-container").classList.remove("active");
      this.active = false;
    },
  },
};
</script>

<style>
.navigation-container {
  padding: 25px 0px;
  background-color: var(--primary-dark);
}

.navigation-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>