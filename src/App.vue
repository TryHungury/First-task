<script setup>
import MovieItem from './components/MovieItem.vue';
import Header from './components/Header.vue'
import Modal from './components/Modal.vue'
import { items } from "./movies.json";
import { ref } from 'vue';

const movies = ref(items)
const showMovieModal = ref(false)
const movieId = ref(null)

function showMovieModalOn() {
  showMovieModal.value = true
}

function showMovieModalOff() {
  showMovieModal.value = false
}

function editMovie(id) {
  movieId.value = id

  showMovieModalOn()
}

function updateMovie(id, movieName, movieDescription, movieImage, movieInTheaters) {
  return movies.value.map((movie) => {
    if (movie.id === id) {
      movie.name = movieName
      movie.description = movieDescription
      movie.image = movieImage
      movie.inTheaters = movieInTheaters
    } else {
      return
    }

    return movie
  })
}

function removeMovie(id) {
  movies.value = movies.value.filter((movie)=>{return movie.id !== id})
}

</script>

<template>
  <div class="w-[51rem] flex flex-col mx-auto">
    <Header :movies="movies"></Header>
    <div class="flex flex-row justify-start flex-wrap mt-4">
      <MovieItem v-for="movie in movies" :key="movie.id" :movie="movie" @edit="editMovie" @remove="removeMovie"/>
    </div>
    <Modal :movies="movies" :movieId="movieId" :showModal="showMovieModal" @modalOff="showMovieModalOff" @editMovieObject="updateMovie"></Modal>
  </div>
</template>
