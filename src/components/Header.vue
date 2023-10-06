<script setup>
import { computed } from 'vue';

const props = defineProps({
  movies: { type: Object, default: null}
})

const totalMovies = computed(() => { return props.movies.length })

const averageRating = computed(() => { 
  let sumRating = 0
  let count = 0

  if (totalMovies.value > 0) {
    props.movies.map((movie) => {
      sumRating += movie.rating
      count += 1
    })

    return sumRating/count
  }
  else {
    return "No Ratings"
  }

})
</script>

<template>
  <div class="flex flex-row justify-between mt-20 h-12 items-center">
    <p class="text-white text-center">Total movies {{ totalMovies }} / Average rating {{ averageRating }}</p>
    <div>
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded transition duration-500 mr-4">
      Remove Ratings
    </button>

    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded transition duration-500">
      Add Movie
    </button>
    </div>
  </div>
</template>