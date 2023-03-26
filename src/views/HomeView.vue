<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0816692">
        <img src="https://cdn.mos.cms.futurecdn.net/xFQ8L37EP3ARaoFg8cQYCD-1200-80.jpg"
        alt="Interstellar Poster" class="featured-img"/>
        <div class="detail">
          <h3>Interstellar</h3>
            <p>A team of explorers travel through a wormhole in space in an attempt to ensure humanity's survival.</p>
      </div>
      </router-link>
    </div>
    <form @submit.prevent="SearchMovies()" class="search-box">
      <input type="text" placeholder="What are you looking for?" v-model="search"> 
      <input type="submit" value="search">
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link v-bind:to="'/movie/' + movie.imdbID" class="movie-link">
        <div class="product-image">
          <img :src="movie.Poster" alt="Movie Poster">
          <div class="type">{{ movie.Type }}</div>
        </div>
        <div class="detail">
          <p class="year">{{ movie.Year }}</p>
          <h3>{{ movie.Title }}</h3>
        </div>
        </router-link>
      </div>
    </div>
    </div>
</template>

<script>
import { ref } from 'vue';
import env from '@/env.js';

export default {
  setup () {
    const search = ref(""); // whatever is typed in box 
    const movies = ref([]); // array of movies

    // called when form is submitted
    const SearchMovies = () => {
      if (search.value != "") {
        fetch(`https://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`) // goes to v-model="search" above, in form
        .then(response => response.json())
        .then(data => {
          movies.value = data.Search; // array of movies
          search.value = ""; // reset
        });
      }
    }

    return {
      search,
      movies,
      SearchMovies
    }
  }
  
}
</script>

<style lang="scss">
.home {
  padding: .5rem;
  .feature-card {
    position: relative;

    .featured-img {
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover; 

      position: relative;
      z-index: 0;
    }

    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;

      h3 {
        color: white;
        margin-bottom: 16px;
      }

      p {
        color: white;
      }
    }
  }

  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 16px;
    align-items: center;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: white;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder {
          color: #f3f3f3;
        }

        &::focus {
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
        }
      }

      &[type="submit"] {
        width: 100%;
        max-width: 250px;
        background-color: #42B883;
        border-radius: 8px;
        color: white;
        font-size: 19px;
        text-transform: uppercase;
        transition: 0.4s;
        padding: 10px;
        letter-spacing: .5px;

        &:hover {
          background-color: #26af7a;
          cursor: pointer;
        }
      }
    }
  }

  .movies-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0px 8px;

    .movie {
      max-width: 50%;
      flex: 1 1 50%;
      padding: 16px 8px;

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;

          img {
            display: block;
            width: 100%;
            height: 275px;
            object-fit: cover;
          }

          .type {
            position: absolute;
            padding: 8px 16px;
            background-color: #42b883;
            color: white;
            bottom: 16px;
            left: 0px;
            text-transform: capitalize;
          }
        }
          .detail {
            background-color: #496583;
            padding: 16px 8px;
            flex: 1 1 100%;
            border-radius: 0px 0px 8px 8px;

            .year {
            color: #AAA;
            font-size: 14px;
            }

            h3 {
              color: white;
              font-weight: 600;
              font-size: 18px;
            }
          }
       }
    }
  }
}

@media only screen and (min-width: 1024px) {
  /* For desktop */
  .home {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    padding-top: 3rem;
    padding-left: 15rem;
    padding-right: 15rem;
   
}

.feature-card {
  width: 100%;
  height: 100%;

  .featured-img {
   height: 65vh;
  }

  h3 {
    font-size: 30px;
  }

  p {
    font-size: 18px;
  }
}

.search-box {
  width: 100%;
  margin-left: 5rem;
  margin-right: 5rem;
  flex-direction: row !important;
  justify-content: space-between !important;

  input[type="text"] {
    margin-bottom: 0 !important; 
    width: 79% !important;
    height: 80% !important;
    min-width: 260px;
  }

  input[type="submit"] {
    margin: 10px;
    padding: 0px !important;
    width: 20% !important;
    height: 80% !important;
    min-width: 110px;
  }
}
} // end desktop view

</style>
