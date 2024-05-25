<template>
  <div class="App font-monospace">
    <div class="content">
      <AppInfo :movieCount="movies.length" :moviesFavouriteCount="movies.filter((c) => c.favourite).length" />
      <div class="search-panel">
        <SearchPanel :updateTermHandle="updateTermHandle" />
        <app-filter :updateFilterHandle="updateFilterHandle" :filterName="filter" />
      </div>

      <Box v-if="!movies.length && !isLoading"><h2 class="text-center fs-3 text-danger">Kinolar yo'q</h2></Box>
      <Box v-else-if="isLoading" class="d-flex justify-content-center">
        <div class="spinner-border" role="status">
          <span class="visually-hidden">Loading...</span>
        </div>
      </Box>
      <movie-list :movies="onFilter(onHandlerSearch(movies, term), filter)" @onToggle="handlerLikeAndFavourite" @remoreMovie="handlerDelete" v-else />
      <Box class="d-flex justify-content-center">
        <nav aria-label="pagination">
          <ul class="pagination pagination-sm page">
            <li v-for="totalPagenNumber in totalPage" :key="totalPagenNumber" :class="{ active: totalPagenNumber === page }" @click="changePage(totalPagenNumber)">
              <span class="page-link">{{ totalPagenNumber }}</span>
            </li>
          </ul>
        </nav>
      </Box>
      <movie-add-form @createMovie="createMovie" />
    </div>
  </div>
</template>

<script>
import AppInfo from "@/components/app-info/AppInfo.vue";
import SearchPanel from "@/components/search-panel/SearchPanel.vue";
import AppFilter from "../app-filter/AppFilter.vue";
import MovieList from "../movie-list/MovieList.vue";
import MovieAddForm from "../movie-add-form/MovieAddForm.vue";
import axios from "axios";
import Box from "@/ui-components/Box.vue";

export default {
  components: {
    AppInfo,
    SearchPanel,
    AppFilter,
    MovieList,
    MovieAddForm,
    Box,
  },
  data() {
    return {
      movies: [],
      term: "",
      filter: "all",
      isLoading: false,
      limit: 10,
      page: 1,
      totalPage: 0,
    };
  },
  methods: {
    async createMovie(item) {
      try {
        const response = await axios.post("https://jsonplaceholder.typicode.com/posts", item);
        this.movies.push(response.data);
      } catch (error) {
        alert(error.message);
      }
    },
    // like and favourite movie
    handlerLikeAndFavourite({ id, prop }) {
      this.movies = this.movies.map((item) => {
        if (item.id == id) {
          return { ...item, [prop]: !item[prop] };
        }
        return item;
      });
    },
    // remove movie
    async handlerDelete(id) {
      try {
        const response = await axios.delete(`https://jsonplaceholder.typicode.com/posts/${id}`);
        this.movies = this.movies.filter((c) => c.id !== id);
      } catch (error) {
        alert(error.message);
      }
    },
    onHandlerSearch(arr, term) {
      if (term.length == 0) {
        return arr;
      }
      return arr.filter((c) => c.name.toLowerCase().indexOf(term) > -1);
    },
    onFilter(arr, filter) {
      switch (filter) {
        case "popular":
          return arr.filter((c) => c.like);
        case "mostViewers":
          return arr.filter((c) => c.viewers > 500);
        default:
          return arr;
      }
    },
    updateFilterHandle(filter) {
      this.filter = filter;
    },
    updateTermHandle(term) {
      this.term = term;
    },
    changePage(page) {
      this.page = page;
    },
    async fetchMovie() {
      try {
        this.isLoading = true;
        const response = await axios.get("https://jsonplaceholder.typicode.com/posts", {
          params: {
            _limit: this.limit,
            _page: this.page,
          },
        });
        const newArr = response.data.map((item) => {
          return {
            name: item.title,
            viewers: item.id * 10,
            favourite: false,
            like: false,
            id: item.id,
          };
        });
        this.movies = newArr;
        this.totalPage = Math.ceil(response.headers["x-total-count"] / this.limit);
      } catch (error) {
        alert(error);
      } finally {
        this.isLoading = false;
      }
    },
  },
  mounted() {
    this.fetchMovie();
  },
  watch: {
    page() {
      this.fetchMovie();
    },
  },
};
</script>

<style>
.page {
  cursor: pointer !important;
}
.App {
  min-height: 100vh;
  color: black;
}
.content {
  max-width: 1000px;
  background-color: #fff;
  margin: auto;
  padding: 5rem 0;
}
.search-panel {
  margin-top: 2rem;
  padding: 1.5rem;
  background-color: #fcfaf5;
  border-radius: 4px;
  box-shadow: 15px 15px 15px rgba(0, 0, 0, 0.15);
}
</style>
