<template>
  <box >
    <h3>Yangi kino qoshish</h3>
    <form class="add-form d-flex gap-2" @submit.prevent>
      <input type="text" placeholder="Qandey kino?" class="form-control new-movie-label" :value="name" @input="name = $event.target.value" />
      <input type="number" placeholder="Nechi marotaba ko'rilgan?" class="form-control new-movie-label" :value="viewers" @input="viewers = $event.target.value" />
      <PrimaryButton type="submit" class="btn-outline-dark" @click="addMovie">Qo'shish</PrimaryButton>
    </form>
  </box>
</template>
<script>
import PrimaryButton from "@/ui-components/PrimaryButton.vue";
import Box from "@/ui-components/Box.vue";
export default {
  data() {
    return {
      name: "",
      viewers: "",
    };
  },
  components: {
    PrimaryButton,
    Box,
  },
  methods: {
    addMovie() {
      if (this.name.trim() == "") {
        alert("Kino nomini kiriting");
      } else if (this.viewers.trim() == "") {
        alert("Nechi marotaba ko'rilganini kiriting");
      } else {
        const newMovie = {
          name: this.name.trim(),
          viewers: this.viewers.trim(),
          favourite: false,
          like: false,
        };
        this.$emit("createMovie", newMovie);
        this.name = "";
        this.viewers = "";
      }
    },
  },
};
</script>

<style></style>
