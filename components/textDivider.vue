<template>
  <div class="text-divider" :style="`transform: rotate(${deg}deg)`">
    <div class="text-divider-text">
      <p>
        <span v-for="i in 4" :key="i">{{ text }}</span>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    text: {
      type: String,
      default: "",
    },
    deg: {
      type: String,
      default: "0",
    },
  },
};
</script>

<style>

</style>