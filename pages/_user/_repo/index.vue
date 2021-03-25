<template>
  <div>
    <v-sheet class="pa-3" v-if="loading">
      <v-skeleton-loader
        class="mx-auto"
        max-width="600"
        type="list-item-avatar, list-item-two-line, actions"
        my-4
        mx-auto
      ></v-skeleton-loader>
    </v-sheet>
    <Repo :repo="repo" v-else-if="!loading && found" />
    <div class="ma-4" v-else-if="!found">
      <h2>{{ errorMsg }}</h2>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      repo: {},
      loading: false,
      found: false,
      errorMsg: "",
    };
  },
  async created() {
    this.loading = true;
    const config = {
      headers: {
        Accept: "application/vnd.github.v3+json",
      },
    };

    try {
      const res = await axios.get(
        `https://api.github.com/repos/${this.$route.params.user}/${this.$route.params.repo}`,
        config
      );
      this.repo = res.data;
      this.found = true;
    } catch (error) {
      if (error.response) {
        console.log("client received an error response (5xx, 4xx)");
        this.errorMsg = "ERROR 404: Repository Not Found";
      } else if (error.request) {
        console.log("client never received a response, or request never left");
        this.errorMsg = "Network Error. Please try again.";
      } else {
        this.errorMsg = "Error occured. Please contact an administrator.";
      }
      console.log(error);
      this.found = false;
    } finally {
      this.loading = false;
    }
  },
};
</script>

<style>
</style>