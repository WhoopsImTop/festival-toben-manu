<template>
  <div>
    <div class="content-container row">
      <div class="text-container col-50">
        <h2>Zahlungs&shy;informationen</h2>
        <p>Die Zahlungsinformationen wurden dir auch per Email gesendet.</p>
        <table style="width: 100%; border-collapse: collapse; margin-top: 20px">
          <tbody>
            <tr style="border-collapse: collapse">
              <th style="border-collapse: collapse; padding: 10px">Ticket</th>
              <th style="border-collapse: collapse; padding: 10px">Anzahl</th>
              <th style="border-collapse: collapse; padding: 10px">Preis</th>
            </tr>
            <tr style="border-collapse: collapse">
              <td style="border-collapse: collapse; padding: 10px">
                Festival Ticket
              </td>
              <td style="border-collapse: collapse; padding: 10px">1</td>
              <td style="border-collapse: collapse; padding: 10px">25.00€</td>
            </tr>
          </tbody>
        </table>
        <p v-if="payment_method == 'PayPal'" style="margin-top: 10px">
          Bitte sende den Betrag an:
          <span
            style="
              font-family: 'canada-type-gibson', sans-serif;
              font-weight: 300;
              color: #c7ff40;
            "
            >stein.torben@gmx.net</span
          >
          per PayPal Freunde und Familie.
        </p>
        <p v-if="payment_method == 'Überweisung'" style="margin-top: 10px">
          Bitte überweise den Gesamtbetrag auf folgendes Konto:<br />
          Kontoinhaber: Torben Stein <br />
          IBAN: DE59 6809 2000 0050 6309 00 <br />
          BIC: GENODE61EMM <br />
          Verwendungszweck: Farsight Festival Vorname Nachname
        </p>
        <p>Vergiss nicht hier unten für deine Genres zu stimmen! 👇</p>
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
      payment_method: "",
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
        .post("https://farsight-festival.de/api/vote", {
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
    this.payment_method = this.$route.query.payment_method;

    axios
      .post("https://farsight-festival.de/api/approve_payment", {
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
    fetch("https://farsight-festival.de/api/votes")
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
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 50px;
  color: var(--primary-light);
  border: 2px solid var(--primary-accent);
}
tr {
  border-bottom: 1px solid var(--primary-accent);
}

th,
td {
  text-align: left;
  padding: 10px;
  font-weight: 300;
  font-family: "IntegralCF", sans-serif !important;
}

th {
  font-weight: 600;
  font-size: 1.2rem;
}
</style>