<template>
  <box>
    <p class="fs-3 text-uppercase">Barcha kinolar soni: {{ movieCount }}</p>
    <p class="fs-4 text-uppercase">Ko'rilgan kinolar soni: {{ moviesFavouriteCount }}</p>
  </box>
</template>
<script>
import Box from "@/ui-components/Box.vue";
export default {
  components: {
    Box,
  },
  props: {
    movieCount: {
      type: Number,
      required: true,
    },
    moviesFavouriteCount: {
      type: Number,
      required: true,
    },
  },
};
</script>
<style></style>