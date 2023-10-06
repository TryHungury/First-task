<script setup>
import { computed, ref } from 'vue';
import { Dialog, DialogPanel, TransitionChild, TransitionRoot } from '@headlessui/vue'

const props = defineProps({
  movies: { type: Object, default: null },
  movieId: {type: Number, default: null},
  showModal: {type: Boolean, default: false}
})

const movieName = ref('')
const movieDescription = ref('')
const movieImage = ref('')
const movieInTheaters = ref(false)

const emit = defineEmits(["modalOff", "editMovieObject"])

const currentMovie = computed(() => {
  return props.movies.find((movie) => { return movie.id === props.movieId })
})

const genres = computed(() => {
  return props.movies.find((movie) => { return movie.id === props.movieId }).genres
})

function showModalOff() {
  emit("modalOff")
}

function movieObject() {
  emit("editMovieObject", currentMovie.value.id, movieName.value, movieDescription.value, movieImage.value, movieInTheaters.value)

  showModalOff()
}

</script>

<template>
  <TransitionRoot as="template" :show="showModal">
    <Dialog as="div" class="relative z-10" @close="showModalOff">
      <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0" enter-to="opacity-100" leave="ease-in duration-200" leave-from="opacity-100" leave-to="opacity-0">
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" />
      </TransitionChild>

      <div class="fixed inset-0 z-10 w-screen overflow-y-auto">
        <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
          <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95" enter-to="opacity-100 translate-y-0 sm:scale-100" leave="ease-in duration-200" leave-from="opacity-100 translate-y-0 sm:scale-100" leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95">
            <DialogPanel class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg">
              <div class="bg-zinc-800 px-4 pb-4 pt-5 sm:p-6 sm:pb-4">
                <form class="sm:flex flex-col sm:items-start">
                  <label class="text-white text-xl" for="inputName"> 
                    Name
                  </label>
                  <input v-model="movieName" class="rounded w-full px-2 mb-2" type="text" id="inputName" name="name" placeholder="Film name"/>
                  <label class="text-white text-xl" for="description"> 
                    Description
                  </label>
                  <textarea  v-model="movieDescription" class="rounded w-full px-2 mb-2" id="description" placeholder="Film description"></textarea>
                  <label class="text-white text-xl" for="image"> 
                    Image
                  </label>
                  <input v-model="movieImage" class="rounded w-full px-2 mb-2" type="url" id="image" name="name" placeholder="Image href"/>
                  <p class="flex flex-row text-white text-xl border-solid border-white border-2 px-2 mb-2 justify-center w-full"> 
                    Genres
                  </p>
                  <label class="flex flex-raw mb-2" v-for="genre in genres">
                    <p class="mr-2 text-white">{{ genre }}</p>
                    <input class="rounded w-full px-2" type="checkbox"/>
                  </label>
                  <label class="text-white flex flex-row w-full border-solid border-white border-2 px-2 justify-center text-xl"> 
                    In Theaters
                    <input v-model="movieInTheaters" class="rounded px-2 ml-2" type="checkbox" checked="false"/>
                  </label>
                </form>
              </div>
              <div class="bg-gray-50 px-4 py-3 sm:flex sm:flex-row-reverse justify-between sm:px-6">
                <button type="button" class="inline-flex w-full justify-center rounded-md bg-yellow-500 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-blue-500 sm:ml-3 sm:w-auto" @click="movieObject">
                  Update
                </button>
                <button type="button" class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:mt-0 sm:w-auto" @click="showModalOff" ref="cancelButtonRef">
                  Cancel
                </button>
              </div>
            </DialogPanel>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>