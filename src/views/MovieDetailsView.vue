<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <back-button />
      </div>
      <div class="row">
        <div class="col">
          <h1>{{ movie.original_title }}</h1>
        </div>
      </div>
      <div class="row">
        <div class="col-12 col-lg-8 fill">
          <img
            :src="'https://image.tmdb.org/t/p/w500' + movie.backdrop_path"
            :alt="movie.original_title"
            class="img-fluid"
          />
        </div>
        <div class="col">
          <h3>Genres</h3>
          <span
            v-for="genre in movie.genres"
            :key="genre"
            class="badge rounded-pill text-bg-primary"
            >{{ genre.name }}</span
          >
        </div>
      </div>
      <div class="row mt-4">
        <div class="col">
          <h2>Related Movies</h2>
          <Pagination :movies="movies" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import BackButton from "@/components/template/BackButton.vue";
import Pagination from "@/components/template/Pagination.vue";
import { mapState } from "vuex";

export default {
  name: "MovieDetails",
  data() {
    return {
      id: this.$route.params.id,
    }
  },
  components: {
    BackButton,
    Pagination,
  },
  computed: {
    ...mapState(["movie", "movies"]),
  },
  beforeMount() {
    this.$store.dispatch("movieDetails", this.$route.params.id);
  },
  watch: {
    $route() {
      if (this.$route.params.id !== this.id || this.$route.params.id !== this.movie.id) {
        this.$store.dispatch("movieDetails", this.$route.params.id);
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/assets/styles/_helpers.scss";
</style>
