<template>
  <div>
    <Loading v-if="$fetchState.pending" />
    <div v-else class="container single-movie">
      <h1>{{ this.movie.original_title }}</h1>
      <NuxtLink class="button" :to="{ name: 'index' }"> Back </NuxtLink>
      <div class="movie-info">
        <div class="movie-img">
          <img
            :src="`https://image.tmdb.org/t/p/w500/${this.movie.poster_path}`"
            alt=""
          />
        </div>
        <div class="movie-content">
          <h1>Title: {{ movie.title }}</h1>
          <p class="movie-fact tagl">
            <span> Tagline: </span>"{{ movie.tagline }}""
          </p>
          <p class="movie-fact">
            <span>Released:</span>
            {{
              new Date(movie.release_date).toLocaleDateString('en-us', {
                year: 'numeric',
                month: 'long',
                day: 'numeric',
              })
            }}
          </p>
          <p class="movie-fact">
            <span>Duration: </span> {{ movie.runtime }} Minutes
          </p>
          <p class="movie-fact"><span>Overview:</span> {{ movie.overview }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Loading from '~/components/Loading.vue'

export default {
  components: { Loading },
  name: 'single-movie',
  head() {
    return {
      title: this.movie.title,
    }
  },
  data() {
    return {
      movie: '',
      api_url: 'https://api.themoviedb.org/3/',
      api_key: '84d4afc4d4fced70ef4f76f2cdf770bd',
    }
  },
  async fetch() {
    await this.getSingleMovie()
  },
  fetchDelay: 1000,
  methods: {
    async getSingleMovie() {
      const data = axios.get(
        this.api_url +
          `movie/${this.$route.params.movieid}?api_key=` +
          this.api_key +
          `&language=en-EN`
      )
      const result = await data
      this.movie = result.data
    },
  },
}
</script>

<style lang="scss">
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;
  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }
  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 500px;
        width: 100%;
        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }
    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }
      .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;
        span {
          font-weight: 600;
          text-decoration: underline;
        }
      }
      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>
