<template>
  <main>
    <!-- immagine iniziale prima di effettuare le ricerche -->
    <div class="welcome" v-if="moviesArray == '' && seriesArray == ''">
      <h1>Inizia la ricerca di film e serie tv!</h1>
    </div>
    <div class="container-fluid d-flex flex-wrap pt-4 justify-content-center">
      <div class="row">
        <!-- non mostra il nome della categoria se non si ottengono risultati dalla ricerca -->
        <h2 v-if="moviesArray != ''" class="text-uppercase pb-3">Film</h2>
        <AppCard
          v-for="(movie, index) in moviesArray"
          :key="index"
          :movie="movie"
        />
      </div>
      <div class="row">
        <!-- non mostra il nome della categoria se non si ottengono risultati dalla ricerca -->
        <h2 v-if="seriesArray != ''" class="text-uppercase pt-5 pb-3">
          Serie TV
        </h2>
        <AppCard
          v-for="(movie, index) in seriesArray"
          :key="index"
          :movie="movie"
        />
      </div>
    </div>
  </main>
</template>

<script>
import AppCard from "./AppCard.vue";

export default {
  name: "AppMain",
  components: {
    AppCard,
  },
  // prelevo gli array dal genitore
  props: {
    moviesArray: Array,
    seriesArray: Array,
  },
};
</script>

<style lang="scss" scoped>
main {
  padding-top: 5rem;
  background-color: #141414;
  font-family: sans-serif;
  color: white;
  .welcome {
    width: 100%;
    height: 100vh;
    background-image: url(../assets/wallpaper.jpg);
    background-repeat: no-repeat;
    background-size: cover;
    position: absolute;
    top: 0;
    left: 0;
    h1 {
      text-transform: uppercase;
      text-align: center;
      vertical-align: middle;
      line-height: 100vh;
    }
  }

  .container-fluid {
    width: 90%;
  }
}
</style>
