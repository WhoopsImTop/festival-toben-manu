<template>
  <div class="content-container full-height">
    <div v-if="!loggedIn" class="login-form">
      <h2>Anmelden</h2>
      <input type="text" placeholder="Email" v-model="email" />
      <input type="password" placeholder="Passwort" v-model="password" />
      <button class="button" @click="login">Anmelden</button>
    </div>
    <div v-else>
      <h2>Anmeldungen</h2>
      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Email</th>
            <th>Bezahlmethode</th>
            <th>Bestelldatum</th>
            <th>Bestell ID</th>
            <th>Bezahlt</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="customer in customers" :key="customer.id">
            <td>{{ customer.name }} {{ customer.last_name }}</td>
            <td>{{ customer.email }}</td>
            <td>{{ customer.payment_method }}</td>
            <td>{{ new Date(customer.created_at).toLocaleString("DE-de") }}</td>
            <td>{{ customer.order_id }}</td>
            <td>
              <button
                @click="updatePayment(true, customer.id)"
                :class="customer.bezahlt ? 'active' : ''"
              >
                Ja
              </button>
              <button
                @click="updatePayment(false, customer.id)"
                :class="customer.bezahlt ? '' : 'active'"
              >
                Nein
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      loggedIn: false,
      customers: [],
      email: "",
      password: "",
    };
  },
  methods: {
    async login() {
      const response = await axios.post(
        "https://farsight-festival.de/api/user/login",
        {
          headers: {
            "Content-Type": "application/json",
            "Access-Control-Allow-Origin": "*",
            "Access-Control-Allow-Credentials": true, // Required for cookies, authorization headers with HTTPS
          },
          body: {
            email: this.email,
            password: this.password,
          },
        }
      );
      if (response.data) {
        this.loggedIn = true;
        this.customers = response.data.customers;
      }
    },
    async updatePayment(bezahlt, id) {
      await axios
        .post("https://farsight-festival.de/api/customer/payment", {
          status: bezahlt,
          customer_id: id,
        })
        .then(() => {
          this.customers = this.customers.map((customer) => {
            if (customer.id === id) {
              customer.bezahlt = bezahlt;
            }
            return customer;
          });
        })
        .catch(() => {
          window.alert(
            "Es gab einen Fehler, bitte versuchen Sie es später erneut."
          );
        });
    },
  },
};
</script>

<style>
.full-height {
  height: 100vh;
  padding: 100px 20px;
}
.login-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 50px 30px;
  background-color: #353535;
  max-width: 500px;
  border-radius: 5px;
  box-shadow: 0px 10px 20px rgba(0, 0, 0, 0.25);
  margin: auto;
}
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 90vh;
}
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

.active {
  background-color: var(--primary-accent);
  border: 1px solid var(--primary-accent);
}

button:hover {
  cursor: pointer;
}
</style>