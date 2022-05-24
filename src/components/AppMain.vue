<template>
  <main>
    <div class="container d-flex flex-wrap pt-4 justify-content-center">
      <div class="row">
        <!-- non mostra il nome della categoria se non si ottengono risultati dalla ricerca -->
        <h2 v-if="moviesArray != ''" class="pb-3">Film</h2>
        <AppCard
          v-for="(movie, index) in moviesArray"
          :key="index"
          :movie="movie"
        />
      </div>
      <div class="row">
        <!-- non mostra il nome della categoria se non si ottengono risultati dalla ricerca -->
        <h2 v-if="seriesArray != ''" class="pt-5 pb-3">Serie TV</h2>
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
}
</style>
