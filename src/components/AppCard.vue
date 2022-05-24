<template>
  <div class="card">
    <div class="card-body">
      <h3 class="card-title">Titolo: {{ movie.title }}</h3>
      <h4 class="card-subtitle mb-2 text-muted">
        Titolo originale: {{ movie.original_title }}
      </h4>
      <h5 class="card-text">Lingua: {{ movie.original_language }}</h5>
      <h5 class="card-text">Voto: {{ movie.vote_average }}/10</h5>
    </div>
  </div>
</template>

<script>
export default {
  name: "AppCard",
};
</script>

<style lang="scss" scoped>
.card {
  border: 2px solid red;
  margin-bottom: 1rem;
  background-color: #141414;
}
</style>
