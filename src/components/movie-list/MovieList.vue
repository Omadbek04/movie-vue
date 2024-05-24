<template>
  <box class="list-group">
    <movie-list-item v-for="movie in movies" v-bind:movie="movie" :key="movie.id" @onToggle="$emit('onToggle', $event)" @remoreMovie="$emit('remoreMovie', $event)" />
  </box>
</template>

<script>
import Box from "@/ui-components/Box.vue";
import MovieListItem from "../movie-list-item/MovieListItem.vue";
export default {
  components: { MovieListItem, Box },

  props: {
    movies: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style scoped></style>
