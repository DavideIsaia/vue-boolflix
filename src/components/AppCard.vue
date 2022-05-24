<template>
  <div class="col-6 col-lg-2 mb-4">
    <div class="card">
      <div class="card-body">
        <h4 class="card-title">{{ movie.title }} {{ movie.name }}</h4>
        <h5 class="card-subtitle mb-2 text-muted">
          Titolo Originale: {{ movie.original_title }} {{ movie.original_name }}
        </h5>
        <h6 class="card-text">
          Lingua:
          <span v-if="!langFlags.includes(movie.original_language)">
            {{ movie.original_language }}
          </span>

          <img
            v-else
            :src="`../assets/flags/${movie.original_language}.png`"
            alt=""
          />
        </h6>
        <h6 class="card-text">Voto: {{ movie.vote_average }}/10</h6>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AppCard",
  data() {
    return {
      langFlags: ["cn", "de", "en", "es", "fr", "it", "ja", "ru", "pt"],
    };
  },
  props: {
    movie: Object,
  },
};
</script>

<style lang="scss" scoped>
.card {
  border: 2px solid red;
  margin-bottom: 1rem;
  background-color: #141414;
}
</style>
