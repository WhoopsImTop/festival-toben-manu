<template>
  <div class="content-container form-bg">
    <div class="row">
      <div class="text-container col-50">
        <h2 style="margin-bottom: 10px">Tickets Kaufen</h2>
        <h3 class="highlighted" style="font-style: italic">20.00€</h3>
        <form>
          <div class="row">
            <div class="col">
              <input
                class="inputField"
                type="text"
                placeholder="Vorname"
                v-model="name"
              />
            </div>
            <div class="col">
              <input
                class="inputField"
                type="text"
                placeholder="Nachname"
                v-model="last_name"
              />
            </div>
          </div>
          <input
            class="inputField"
            type="text"
            placeholder="E-Mail"
            v-model="email"
          />
          <h4 style="color: #efefef; letter-spacing: 1px; margin-top: 10px; font-weight: 400">Zahlungsmethode</h4>
          <div class="row">
            <div
              class="payment_method"
              :class="paypal ? 'active' : ''"
              @click="paypal = true"
            >
              PayPal
            </div>
            <div
              class="payment_method"
              :class="paypal ? '' : 'active'"
              @click="paypal = false"
            >
              Überweisung
            </div>
          </div>
          <label style="display: flex; align-items: center"
            ><input type="checkbox" v-model="datenschutz" />Ich bin mit der
            Verarbeitung meiner daten gemäss datenschutzerklärung
            einverstanden.</label
          >
          <button
            class="button"
            :disabled="!datenschutz"
            @click="redirectToPaymentPage"
          >
            {{ buttonText }}
          </button>
          <p class="hint">Ticketverkauf startet bald</p>
        </form>
      </div>
      <div class="col-50">
        <img src="ticket.svg" alt="ticket" class="ticket" />
      </div>
    </div>
    <span class="info-text"
      >Das Farsight Festival ist eine private Veranstaltung. Nur Personen aus
      dem Freundes und Familienkreis sind zu diesem Festival eingeladen.<br />
      Tickets werden ausschließlich verkauft, um die Kosten zu decken. Das
      Festival spielt keine Gewinne ein.</span
    >
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      name: "",
      last_name: "",
      email: "",
      datenschutz: false,
      paypal: true,
      buttonText: "Ticket Kaufen",
    };
  },
  methods: {
    redirectToPaymentPage(e) {
      this.buttonText = "Bitte warten...";
      e.preventDefault();
      if (!this.validateForm()) {
        document.querySelectorAll(".inputField").forEach((input) => {
          if (input.value.length === 0) {
            input.classList.add("invalid");
          }
        });
        return;
      }
      let payment_method = this.paypal ? "PayPal" : "Überweisung";
      axios
        .post("https://mein-campusplan.de/checkout", {
          name: this.name,
          last_name: this.last_name,
          email: this.email,
          lineItems: [
            {
              id: "01",
              name: "Festival Ticket",
              price: "20.00",
              quantity: 1,
            },
          ],
          payment_method,
          total: 20.00,
        })
        .then((res) => {
          window.location.href = res.data.url;
        })
        .catch((err) => {
          window.alert("Es ist ein Fehler aufgetreten, bitte versuchen Sie es später erneut.");
          this.buttonText = "Ticket Kaufen";
        });
    },
    validateForm() {
      return (
        this.name.length > 0 &&
        this.last_name.length > 0 &&
        this.email.length > 0
      );
    },
  },
};
</script>

<style>
.invalid {
  border: 1px solid red !important;
}

.payment_method {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 48.5%;
  padding: 30px;
  border-radius: 2px;
  margin-top: 10px;
  color: var(--primary-light);
  border: 1px solid var(--primary-accent);
}

.payment_method.active {
  background-color: var(--primary-accent);
  color: var(--primary-dark);
}
</style>