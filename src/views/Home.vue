<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0112159">
        <img src="https://m.media-amazon.com/images/M/MV5BYjY1Y2ZmNDctZWQ3Yy00MTE3LTk2M2QtMjQ0MDA5ODVjMDEyXkEyXkFqcGdeQXVyNTA4NzY1MzY@._V1_SX300.jpg" 
        alt="Evangelion_poster"
        class="featured-img">
        <div class="detail">
          <h3>Neon Genesis Evangelion</h3>
          <p>A teenage boy finds himself recruited as a member of an elite team of pilots by his father</p>
        </div>
      </router-link>
    </div>

    <form @submit.prevent="SearchMovies()" class="search-box">
      <input type="text" placeholder="¿Qué buscamos?" v-model="search" />
      <input type="submit" value="Buscar">
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbId">
        <router-link v-bind:to="'/movie'+movie.imdbId" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie Poster"/>
            <div class="type">{{movie.Type}}</div>
          </div>
          <div class="detail">
            <p class="year">{{movie.Year}}</p>
            <h3>{{movie.Title}}</h3>
          </div>

        </router-link>
      </div>
    </div>

  </div>
</template>

<script>
// @ is an alias to /src
import {ref} from 'vue';
import env from '@/env.js';


export default {
  setup() {
    const search = ref("");
    const movies = ref([]);

    const SearchMovies = () => {
      if (search.value != "") {
        fetch(`https://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
        .then(response => response.json()) 
        .then(data => {
          movies.value = data.Search;
          search.value = "";          
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
.home{
  .feature-card{
    position: relative;

    .featured-img{
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover; 
      position: relative; 
      z-index: 0;  
    }

    .detail{
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0,0,0,0.6);
      padding:16px;
      z-index: 1;

      h3{
      color: #fff;
      margin-bottom: 16px;
      }

      p{
        color:#fff;

      }
    }
  }
  .search-box{
    display:flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input{
      display: black;
      appearance: none;
      border:none;
      outline:none;
      background: none;

      &[type="text"] {
        width: 100%;
        color:white;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: .4s;

        &::placeholder{
          color:#cccccc;
          font-size: 17px;
        }
        
        &:focus{
          box-shadow: 0px 3px 6px rgba(0,0,0,0.2);
        }
      }
      &[type="submit"]{
        width: 100%;
        max-width: 300px;
        background-color: transparent;
        border:2px solid #42B883;
        padding: 16px;
        border-radius: 8px;
        color: white;
        font-size: 18px;
        transition: .4s;
        text-transform: uppercase;        
      }
    }
  }

  .movies-list{
    display: flex;
    flex-wrap: wrap;
    margin:0px 8px;

    .movie{
      max-width: 50%;
      flex: 1 1 50%;
      padding: 16px 8px;

      .movie-link{
        display:flex;
        flex-direction: column;
        height: 100%;

        .product-image{
          position: relative;
          display: block;

          img {
          width: 100%;
          height: 275px; 
          display: block;
          object-fit: cover;     
          }

          .type{
            position: absolute;
            padding: 8px 16px;
            background-color: #42b883;
            color: #fff;
            bottom: 16px;
            left: 0px;
            text-transform: capitalize;

          }
        }
        .detail{
            background-color: #496583;
            padding: 16px 8px;
            flex: 1 1 100%;
            border-radius: 0px 0px 8px 0px;

            .year{
              color:#AAA;
              font-size:14px;
            }

            h3{
              color:#fff;
              font-weight: 600px;
              font-size: 18px;
            }

          }
      }

    }
  }
}
</style>
