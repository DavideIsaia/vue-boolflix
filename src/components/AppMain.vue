<template>
  <main>
    <div class="container d-flex flex-wrap pt-4 justify-content-center">
      <div class="row">
        <h2>Film</h2>
        <AppCard
          v-for="(movie, index) in moviesArray"
          :key="index"
          :movie="movie"
        />
      </div>
      <div class="row">
        <h2>Serie TV</h2>
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
  props: {
    moviesArray: Array,
    seriesArray: Array,
  },
};
</script>

<style lang="scss" scoped>
main {
  background-color: #141414;
  font-family: sans-serif;
  color: white;
}
</style>
