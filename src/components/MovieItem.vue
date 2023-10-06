<script setup>
import { computed } from 'vue';
import { PencilIcon, StarIcon, TrashIcon } from "@heroicons/vue/24/solid";

const props = defineProps({
  movie: {
    type: Object, default: null}
})

const emit = defineEmits(["edit", "remove", "update:rating"]);

const valueStars = computed(() => {
  const ratingArray = []
  
  for (let i = 0; i < props.movie.rating; i++) {
    ratingArray.push(i)
  }

  return ratingArray
})

const blindStars = computed(() => {
  const blindArray = []
  const computedValue = 5 - props.movie.rating

  for (let i = 0; i < computedValue ; i++) {
    blindArray.push(i)
  }

  return blindArray
})

function clickEdit(id) {
  emit("edit", id);
}

function clickDelete(id) {
  emit("remove", id);
}

</script>

<template>
  <div class="w-64 m-2 rounded-xl relative group hover:opacity-100">
    <img :src="movie.image" class="rounded object-cover h-96"/>
    <div class="p-2 bg-white rounded-ee rounded-es h-44">
      <p class="text-base">
        {{ movie.name }}
      </p>
      <div class="flex flex-row flex-wrap">
        <p class="rounded-xl bg-indigo-500 text-white text-xs px-2 py-0.5 mr-1" v-for="genre in movie.genres" :key="genre">
          {{ genre }}
        </p>
      </div>
      <div class="overflow-hidden max-w-full h-20 mt-2">
        <p class="text-xs block overflow-hidden">
          {{ movie.description }}
        </p>
      </div>
      <div class="absolute bottom-3 left-2 flex flex-row flex-wrap">
        <p class="text-xs mr-2 mt-2">
          Rating: {{ movie.rating }}/5
        </p>
        <StarIcon v-for="valueStar in valueStars" class="w-4 fill-yellow-500 mt-2"></StarIcon>
        <StarIcon v-for="blindStar in blindStars" class="w-4 fill-stone-900 mt-2"></StarIcon>
      </div>
    </div>
    <StarIcon class="w-8 fill-yellow-500 absolute top-0 right-0"></StarIcon>
    <p class="text-base absolute top-1 right-3">
      {{ movie.rating }}
    </p>
    <div class="w-24 h-7 absolute bottom-2 right-0 flex flex-row justify-evenly opacity-0 group-hover:opacity-100 transition duration-300">
      <button @click="clickEdit(movie.id)" class="bg-slate-300 hover:bg-purple-500 hover:text-white w-1/3 h-full rounded-3xl transition duration-500 flex flex-row justify-center items-center">
        <PencilIcon class="w-4 h-4 text-center" />
      </button>
      <button @click="clickDelete(movie.id)" class="bg-slate-300 hover:bg-red-500 hover:text-white w-1/3 h-full rounded-3xl transition duration-500 text-center flex flex-row justify-center items-center">
        <TrashIcon class="w-4 h-4 text-center" />
      </button>
    </div>
  </div>
</template>