<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <span>{{ screenwidth }}</span>
      </div>
    </div>
    <div class="row">
      <div class="col-12 col-lg-3 my-2" v-for="movie in movies" :key="movie">
        <div class="card text-white">
          <img
            class="card-img img-fluid card-img-top"
            :src="movie.poster_path?'https://image.tmdb.org/t/p/w500' + movie.poster_path: 'https://protkd.com/wp-content/uploads/2017/04/default-image.jpg'"
            alt="Card image"
          />
          <div class="card-img-overlay">
            <h2 v-if="screenwidth>=550" class="card-title"> {{ movie.original_title }}</h2>
            <h5 v-else class="card-title"> {{ movie.original_title }}</h5>
            <p v-if="screenwidth>=550" class="card-text">
              {{
                movie.overview.length > 100
                  ? movie.overview.substring(0, 100) + "..."
                  : movie.overview
            }}
            </p>
            <a @click="change_movie(movie)" :key='$route.fullPath' class="btn btn-outline-light">Find out</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      screenwidth: 0,
    }
  },
  props: {
    movies: {
      type: Array,
      required: true,
    },
  },
  methods: {
    change_movie(movie) {
      this.$router.push({
        name: "movie",
        params: {
          id: movie.id,
        },
      });
    },
  },
  mounted() {
    this.screenwidth = parseInt(window.screen.availWidth);
    window.addEventListener("resize", () => {
      this.screenwidth = parseInt(window.screen.availWidth);
    });
  },
};
</script>

<style lang="scss">
@import "@/assets/styles/_variables.scss";

.card-img-overlay {
  transform: translateY(13rem);
  transition: transform 0.18s ease-in;
  background: linear-gradient(0deg, rgba($info,1) 0%, rgba($info,0.8) 62%, rgba($info,0.3) 100%);
  .card-text {
    opacity: 0;
    transform: translateY(-6rem);
    transition: opacity 0.05s ease-in, transform 0.25s ease-in;
  }
  .btn {
    opacity: 0;
    transform: translateY(2rem);
  }
}
.card {
  overflow: hidden;
  background-color: #345;
  &:hover {
    .card-img {
      mix-blend-mode: overlay;
    }
    .card-img-overlay {
      transform: translateY(0);
      transition: transform 0.25s ease-out;
      background-color: rgba(0, 0, 0, 0.1);
    }
    .card-text {
      opacity: 1;
      transform: translateX(0);
      transition: opacity 0.5s ease-out, transform 0.2s ease-out;
    }
    .btn {
      opacity: 1;
      transform: translateX(0);
      transition: opacity 0.7s ease-out, transform 0.35s ease-out;
    }
  }
}

.card-img-top {
    width: 100%;
    height: 30vw;
    object-fit: cover;
}
</style>