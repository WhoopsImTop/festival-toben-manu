<template>
  <div class="content-contaner">
    <h2>Genres und ihre Stimmen</h2>
    <div class="vote-row" style="margin-bottom: 40px">
      <div
        class="col vote-container"
        v-for="vote in voteableData"
        :key="vote.id"
      >
        <span>{{ vote.genre }}</span>
        <div class="icon-container">
          <div
            class="vote-icon"
            v-for="i in vote.votes <= votesToShow ? vote.votes : votesToShow"
            :key="i"
            :style="
              'left:' +
              (-leftMargin + i * leftMargin) +
              'px; z-index:' +
              (100 - i)
            "
          >
            <span> ❤️ </span>
          </div>
          <div
            v-if="vote.votes > votesToShow"
            :style="
              'z-index: 95; left:' +
              (-leftMargin + (votesToShow + 1.2) * leftMargin + 2) +
              'px'
            "
            class="vote-icon"
          >
            <span>+{{ vote.votes - votesToShow }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      voteableData: [],
    };
  },
  computed: {
    leftMargin() {
      if (window.innerWidth < 600) return 14;
      return 20;
    },
    votesToShow() {
      if (window.innerWidth < 600) return 2;
      return 4;
    },
  },
  beforeMount() {
    fetch("https://mein-campusplan.de/votes")
      .then((res) => res.json())
      .then((data) => {
        this.voteableData = data.votes;
        console.log(this.voteableData);
      })
      .catch((err) => {
        window.alert(
          "Es ist ein Fehler aufgetreten. Bitte versuche es später noch einmal."
        );
      });
  },
};
</script>

<style>
</style>