<template>
  <main>
    <div class="container d-flex flex-wrap pt-5 justify-content-center">
      <div class="row">
        <div
          class="col-6 col-lg-2 mb-4"
          v-for="(movie, index) in moviesArray"
          :key="index"
        >
          <!-- <AppCard /> -->
          <div class="card">
            <div class="card-body">
              <h4 class="card-title">{{ movie.title }}</h4>
              <h5 class="card-subtitle mb-2 text-muted">
                Titolo originale: {{ movie.original_title }}
              </h5>
              <h6 class="card-text">Lingua: {{ movie.original_language }}</h6>
              <h6 class="card-text">Voto: {{ movie.vote_average }}/10</h6>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
// import AppCard from "./AppCard.vue";

export default {
  name: "AppMain",
  // components: AppCard,
  props: {
    moviesArray: Array,
  },
};
</script>

<style lang="scss" scoped>
main {
  background-color: #141414;
  font-family: sans-serif;
  color: white;
  .card {
    border: 2px solid red;
    margin-bottom: 1rem;
    background-color: #141414;
  }
}
</style>
