<template>
  <div>
    <div class="content-container row">
      <div class="text-container col-50">
        <h2>Wir freuen uns auf dich!</h2>
        <p>
          Du hast dich erfolgreich für das Farsight Festival angemeldet.<br />
          Wir freuen uns auf dich und hoffen, dass du ein tolles Erlebnis hast.
        </p>
        <p>Wenn du Fragen hast, kannst du uns gerne kontaktieren.</p>
      </div>
      <div class="col-50">
        <img src="/ticket.svg" alt="ticket" class="ticket" />
      </div>
    </div>
    <div class="content-container">
      <hr style="width: 100%; border: 1px solid #efefef" />
      <h2 style="margin-top: 40px">
        Bevor du gehst, hier kannst du noch für Genres abstimmen
      </h2>
      <h3 style="margin-bottom: 20px">
        Du kannst für {{ votesToUse - currentVotes }} Genres abstimmen
      </h3>
      <div class="vote-row">
        <div
          class="col vote-container"
          :class="userVotes.includes(vote.id) ? 'vote-active' : ''"
          v-for="vote in voteableData"
          :key="vote.id"
          @click="addToVoteArray(vote.id)"
        >
          <span>{{ vote.genre }}</span>
        </div>
      </div>
    </div>
    <div class="content-container">
      <button class="button" @click="submitVotes">Abstimmen</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      cid: "",
      payerId: "",
      voteableData: [],
      userVotes: [],
      votesToUse: 5,
    };
  },
  computed: {
    currentVotes() {
      return this.userVotes.length;
    },
  },
  methods: {
    addToVoteArray(vote) {
      if (this.userVotes.includes(vote)) {
        this.userVotes.splice(this.userVotes.indexOf(vote), 1);
      } else {
        if (this.currentVotes == this.votesToUse) {
          return window.alert(
            "Du kannst nur für " + this.votesToUse + " Genres abstimmen"
          );
        }
        this.userVotes.push(vote);
      }
    },
    submitVotes() {
      axios
        .post("https://mein-campusplan.de/vote", {
          customer_id: this.cid,
          votes: this.userVotes,
        })
        .then(() => {
          this.$router.push("/dankeschoen");
        })
        .catch((err) => {
          console.log(err);
          window.alert(
            "Es ist ein Fehler aufgetreten. Bitte versuche es später noch einmal."
          );
        });
    },
  },
  beforeMount() {
    this.cid = this.$route.query.cid;
    this.payerId = this.$route.query.PayerID;

    axios
      .post("https://mein-campusplan.de/approve_payment", {
        customer_id: this.cid,
        payment_id: this.payerId,
      })
      .then((res) => {
        console.log(res);
      })
      .catch((err) => {
        console.log(err);
      });
  },
  mounted() {
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