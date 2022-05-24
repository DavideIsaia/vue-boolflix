<template>
  <div class="col-6 col-md-4 col-lg-3 col-xl-2 mb-4">
    <div class="card">
      <div class="card-body">
        <img
          class="card-img-top"
          :src="`https://www.themoviedb.org/t/p/w342${movie.poster_path}`"
        />
        <h4 class="card-title mt-2">{{ movie.title }} {{ movie.name }}</h4>
        <h5 class="card-subtitle text-muted mb-2">
          Titolo Originale: {{ movie.original_title }} {{ movie.original_name }}
        </h5>
        <h6 class="card-text">
          Lingua:
          <span v-if="!langFlags.includes(movie.original_language)">
            {{ movie.original_language }}
          </span>

          <img
            v-else
            class="flag"
            :src="require(`../assets/flags/${movie.original_language}.png`)"
            :alt="movie.original_language"
          />
        </h6>
        <h6 class="card-text">
          Voto:
          <i
            v-for="index in starsVote(movie.vote_average)"
            :key="index"
            class="fas fa-star"
          >
          </i>
        </h6>
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
  methods: {
    starsVote(vote) {
      return Math.round(vote / 2);
    },
  },
};
</script>

<style lang="scss" scoped>
.card {
  border: 2px solid red;
  margin-bottom: 1rem;
  background-color: #141414;

  .flag {
    height: 1.5rem;
  }

  .fa-star {
    color: yellow;
    display: inline;
    margin-left: 0.1rem;
  }
}
</style>
