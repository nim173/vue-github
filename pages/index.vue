<template>
  <div>
    <SearchRepo @search-text="searchText($event, PAGE_ONE)" class="my-4" />
    <div class="mx-auto overflow-hidden px-2" v-if="displayResults">
      <h2>{{ totalCount }} repository results found for "{{ text }}":</h2>
    </div>
    <v-sheet class="pa-3" v-if="loading">
      <v-skeleton-loader
        v-for="n in 5"
        :key="n"
        class="mx-auto"
        max-width="600"
        type="list-item-avatar, list-item-two-line, actions"
        my-4
        mx-auto
      ></v-skeleton-loader>
    </v-sheet>

    <div class="d-flex flex-wrap">
      <Repo class="col-5" v-for="repo in repos" :key="repo.id" :repo="repo" />
    </div>

    <div class="text-center" v-if="displayResults && !loading">
      <v-pagination
        v-model="page"
        :length="numPages"
        :total-visible="7"
      ></v-pagination>
    </div>

    <div v-if="!displayResults">
      <h2>{{ errorMsg }}</h2>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import SearchRepo from "../components/SearchRepo";
import Repo from "../components/Repo";

export default {
  name: "Home",
  components: {
    SearchRepo,
    Repo,
  },
  data() {
    return {
      repos: [],
      text: "",
      totalCount: 0,
      displayResults: false,
      page: 1,
      PAGE_ONE: 1,
      PAGE_SIZE: 12,
      loading: false,
      errorMsg: "",
    };
  },
  methods: {
    async searchText(text, page) {
      if (text !== "") {
        this.loading = true;
        this.repos = [];
        this.page = page;
        const config = {
          headers: {
            Accept: "application/vnd.github.v3+json",
          },
        };

        try {
          const res = await axios.get(
            `https://api.github.com/search/repositories?q=${text}&page=${page}&per_page=${this.PAGE_SIZE}`,
            config
          );
          this.repos = res.data.items;
          this.totalCount = res.data.total_count;
          // display results
          this.text = text;
          this.displayResults = true;
        } catch (error) {
          if (error.response) {
            console.log("client received an error response (5xx, 4xx)");
            this.errorMsg = "Error 404: Not Found";
          } else if (error.request) {
            console.log(
              "client never received a response, or request never left"
            );
            this.errorMsg = "Network Error. Please try again.";
          } else {
            this.errorMsg = "Error occured. Please contact an administrator.";
          }
          console.log(error);
        } finally {
          this.loading = false;
        }
      } else {
        // clear search results
        this.displayResults = false;
        this.repos = [];
        console.log("empty search");
      }
    },
  },
  computed: {
    numPages() {
      if (this.totalCount == 0) {
        return 0;
      } else {
        return Math.ceil(this.totalCount / this.PAGE_SIZE);
      }
    },
  },
  watch: {
    page: function (val) {
      this.searchText(this.text, val);
    },
  },
};
</script>
