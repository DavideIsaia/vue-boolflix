<template>
  <div class="col-12 col-md-6 col-lg-4 col-xl-3 mb-4">
    <div class="card">
      <img
        class="card-img-top"
        :src="`https://www.themoviedb.org/t/p/w342${movie.poster_path}`"
      />
      <div class="card-body">
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
        <small>
          {{ movie.overview }}
        </small>
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
  position: relative;
  background-color: #060606;
  width: 100%;
  height: 100%;
  cursor: pointer;

  .flag {
    height: 1.5rem;
  }

  .fa-star {
    color: gold;
    display: inline;
    margin-left: 0.1rem;
  }

  .card-body {
    display: none;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  &:hover .card-body {
    display: block;
    background-color: rgba($color: #000000, $alpha: 0.8);
  }
}
</style>
