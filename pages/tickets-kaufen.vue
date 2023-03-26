<template>
  <div class="content-container form-bg">
    <div class="row">
      <div class="text-container col-50">
        <h2 style="margin-bottom: 10px">Tickets Kaufen</h2>
        <h3 class="highlighted" style="font-style: italic">20.00€</h3>
        <form>
          <div class="row">
            <div class="col">
              <input type="text" placeholder="Vorname" />
            </div>
            <div class="col">
              <input type="text" placeholder="Nachname" />
            </div>
          </div>
          <input type="text" placeholder="E-Mail" />
          <label style="display: flex; align-items: center"
            ><input type="checkbox" />Ich bin mit der Verarbeitung meiner daten
            gemäss datenschutzerklärung einverstanden.</label
          >
          <button class="button" disabled>Ticket Kaufen</button>
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
export default {};
</script>

<style>
</style>