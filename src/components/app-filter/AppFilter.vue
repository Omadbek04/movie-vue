<template>
  <div class="btn btn-group filter-movie">
    <PrimaryButton 
    v-for="filter in buttonsData"
    :key="filter.name" type="button" 
    @click="filterHandler(filter.view)" 
    :class="[filterName == filter.view ? 'btn-dark' : 'btn-outline-dark']">
    {{ filter.name }}
  </PrimaryButton>
  </div>
</template>
<script>
import PrimaryButton from '@/ui-components/PrimaryButton.vue';
export default {
  data() {
    return {
      buttonsData: [
        {
          name: "Barcha kinolar",
          view: "all",
        },
        {
          name: "Mashhur kinolar",
          view: "popular",
        },
        {
          name: "Eng kop korilgan kinolar",
          view: "mostViewers",
        },
      ],
      filter: "all",
    };
  },
  components:{
    PrimaryButton,
  },
  props: {
    updateFilterHandle: {
      trype: Function,
      required: true,
    },
    filterName: {
      type: String,
      required: true,
    },
  },
  methods: {
    filterHandler(filter) {
      this.filter = filter;
      this.updateFilterHandle(this.filter);
    },
  },
};
</script>
<style scoped></style>
