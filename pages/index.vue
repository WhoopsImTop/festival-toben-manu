<template>
  <div>
    <div class="landing-container">
      <video class="video-autoplay" autoplay muted loop>
        <source src="/farsight.mp4" type="video/mp4" />
      </video>
      <div class="content-container">
        <div class="notice-banner">
          <span>01.07.2023</span>
        </div>
        <h1>Farsight<br />Festival<br />2023</h1>
      </div>
    </div>
    <div class="content-container">
      <div class="text-container col-70">
        <h2>Das Farsight Festival</h2>
        <p>
          Willkommen zum Farsight Festival, einem einzigartigen Festivalerlebnis
          auf einem Berg mit atemberaubender Aussicht auf den majestätischen
          Berg Kandel! Dieses Festival ist für alle Musik- und
          Naturbegeisterten, die auf der Suche nach einem besonderen Erlebnis
          sind. Das Farsight Festival bietet Ihnen nicht nur ein vielfältiges
          Musikprogramm, sondern auch eine spektakuläre Aussicht, die Sie für
          immer in Erinnerung behalten werden.
        </p>
        <br />
        <span class="small-notice-banner">Text Sponsored by ChatGPT</span>
      </div>
      <h2>Trailer</h2>
      <video class="small-video" loop controls style="margin-bottom: 100px;">
        <source src="/farsight.mp4" type="video/mp4" />
      </video>
      <genres />
    </div>
    <text-divider text="Veranstalter" deg="-2" />
    <div class="content-container">
      <div class="text-container row">
        <div class="card">
          <img src="/torben.jpeg" alt="Torben" />
          <h3 style="font-weight: bold">Torben Stein</h3>
        </div>
        <div class="card">
          <img src="/manu.jpeg" alt="Torben" />
          <h3 style="font-weight: bold">Manuel Oesterle</h3>
        </div>
      </div>
    </div>
    <text-divider text="Disclamer" deg="3" />
    <div class="content-container">
      <div class="text-container">
        <p>
          Bitte beachten Sie, dass dieses Festival ein privates Event ist, das
          nicht öffentlich beworben oder beworben wird. Als solches ist keine
          GEMA-Lizenz erforderlich, um Musik abzuspielen. Das bedeutet, dass
          alle auf dem Festival gespielten Songs von den Künstlern oder
          Plattenfirmen lizenziert wurden oder als gemeinfrei gelten. Wir
          möchten betonen, dass wir keine kommerzielle Veranstaltung sind und
          keinerlei Einkünfte aus dem Festival erzielen. Wir laden unsere
          Freunde und Familie ein, gemeinsam eine großartige Zeit zu haben und
          Musik zu genießen. Wir bitten alle Teilnehmer, sich entsprechend zu
          verhalten und die Urheberrechte zu respektieren. Wenn Sie weitere
          Fragen haben oder Bedenken bezüglich des Festivals haben, wenden Sie
          sich bitte an uns. Vielen Dank für Ihr Verständnis und Ihre
          Unterstützung.
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import genres from "../components/genres.vue";
export default {
  components: { genres },
  name: "IndexPage",
};
</script>

<style></style>
