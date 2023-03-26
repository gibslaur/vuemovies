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

<style>
</style>
