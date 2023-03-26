<template>
  <div class="text-container">
    <p class="stagger" v-for="i in 10" :key="i">{{ text }}</p>
  </div>
</template>

<script>
import { gsap } from "gsap";
import SplitTextJS from "split-text-js";

export default {
  props: {
    text: {
      type: String,
      default: "",
    },
  },
  methods: {
    animateText() {
      const textToSplit = gsap.utils.toArray(".stagger");
      let tl = gsap.timeline();
      textToSplit.forEach((text) => {
        const splitText = new SplitTextJS(text, {
          type: "chars",
          charsClass: "char-child",
        });
        const chars = splitText.chars;
        const animation = gsap.from(
          chars,
          {
            opacity: 0,
            y: 80,
            rotateX: -90,
            stagger: 0.05,
          },
          "<"
        ).to(
          chars,
          {
            opacity: 0,
            y: -80,
            rotateX: 90,
            stagger: 0.05,
          },
          "<1"
        );
        //restart animation one animation before it ends
        tl.eventCallback("onUpdate", () => {
          if (tl.progress() > 0.9) {
            tl.restart();
          }
        });
      });
    },
  },
  created() {
    if (!process.server) {
      setTimeout(() => {
        this.animateText();
      }, 200);
    }
  },
};
</script>

<style scoped>
.text-container {
  margin: 100px 0;
}
.stagger {
  font-size: 100px;
  font-weight: 900;
  text-transform: uppercase;
  text-align: center;
  margin: 0;
  line-height: 0;
}
</style>