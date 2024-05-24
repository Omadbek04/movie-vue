<template>
  <div class="App font-monospace">
    <div class="content">
      <AppInfo :movieCount="movies.length" :moviesFavouriteCount="movies.filter((c) => c.favourite).length" />
      <div class="search-panel">
        <SearchPanel :updateTermHandle="updateTermHandle" />
        <app-filter :updateFilterHandle="updateFilterHandle" :filterName="filter" />
      </div>
      <movie-list :movies="onFilter(onHandlerSearch(movies, term), filter)" @onToggle="handlerLikeAndFavourite" @remoreMovie="handlerDelete" />
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

export default {
  components: {
    AppInfo,
    SearchPanel,
    AppFilter,
    MovieList,
    MovieAddForm,
  },
  data() {
    return {
      movies: [
        {
          id: 1,
          name: "Omar",
          viewers: 881,
          favourite: false,
          like: true,
        },
        {
          id: 2,
          name: "Erteglu",
          viewers: 780,
          favourite: false,
          like: false,
        },
        {
          id: 3,
          name: "Emperie of osman",
          viewers: 222,
          favourite: true,
          like: false,
        },
      ],
      term: "",
      filter: "all",
    };
  },
  methods: {
    createMovie(item) {
      this.movies.push(item);
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
    handlerDelete(id) {
      this.movies = this.movies.filter((c) => c.id !== id);
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
  },
};
</script>

<style>
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
