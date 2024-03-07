<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0409591">
        <img
          src="https://m.media-amazon.com/images/I/61ssElbI-ML._AC_SY679_.jpg"
          alt="Naruto poster"
          class="feature-img"
        />
        <div class="detail">
          <h3>Naruto</h3>
          <p>
            Naruto Uzumaki, a mischievous adolescent ninja, struggles as he
            searches for recognition and dreams of becoming the Hokage, the
            village's leader and strongest ninja.
          </p>
        </div>
      </router-link>
    </div>

    <form @submit.prevent="searchMovies" class="search-box">
      <input
        type="text"
        placeholder="What are you looking for?"
        v-model="search"
      />
      <input type="submit" value="search" />
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="`/movie/${movie.imdbID}`" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie Poster" />
            <div class="type">{{ movie.Type }}</div>
          </div>
          <div class="detail">
            <p class="y">{{ movie.Year }}</p>
            <h2 class="t">{{ movie.Title }}</h2>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import env from "@/env.js";

export default {
  setup() {
    const search = ref("");
    const movies = ref([]);

    const searchMovies = () => {
      if (search.value !== "") {
        fetch(`http://www.omdbapi.com/?apikey=${env.apiKey}&s=${search.value}`)
          .then((response) => response.json())
          .then((data) => {
            if (data.Search) {
              movies.value = data.Search;
              search.value = "";
            } else {
              console.log("No movies found");
            }
          })
          .catch((error) => {
            console.error("Error fetching movies:", error);
          });
      }
    };

    return {
      search,
      movies,
      searchMovies,
    };
  },
};
</script>

<style scoped lang="scss">
/* Your scoped styles go here */
.home {
  .feature-card {
    position: relative;
    margin-bottom: 20px;
  }

  .feature-img {
    display: block;
    width: 100%;
    height: auto;
    object-fit: cover;
  }

  .detail {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.6);
    padding: 10px;
    color: #fff;

    h3 {
      font-size: 18px;
      margin-bottom: 5px;
    }

    p {
      font-size: 14px;
      line-height: 1.3;
    }
  }
}

.search-box {
  display: flex;
  margin-bottom: 20px;

  input[type="text"] {
    flex: 1;
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 5px 0 0 5px;
  }

  input[type="submit"] {
    padding: 10px 20px;
    background-color: #42b883;
    color: #fff;
    border: none;
    border-radius: 0 5px 5px 0;
    cursor: pointer;
  }
}

.movies-list {
  display: flex;
  flex-wrap: wrap;

  .movie {
    width: 50%;
    padding: 10px;
    box-sizing: border-box;

    .movie-link {
      display: flex;
      flex-direction: column;
      height: 100%;
      text-decoration: none;

      .product-image {
        position: relative;

        img {
          width: 100%;
          height: auto;
          object-fit: cover;
        }

        .type {
          position: absolute;
          bottom: 0;
          left: 0;
          background-color: #42b883;
          color: #fff;
          padding: 5px 10px;
          font-size: 14px;
          text-transform: capitalize;
        }
      }

      .detail {
        background-color: #496583;
        color: #fff;
        padding: 10px;

        .y {
          font-size: 14px;
          color: #aaa;
          margin-bottom: 5px;
        }

        .t {
          font-size: 18px;
          margin: 0;
        }
      }
    }
  }
}
</style>
