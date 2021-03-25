<template>
  <form @submit.prevent="submit">
    <v-text-field
      v-model="search"
      :rules="rules"
      counter="256"
      hint="Search for GitHub repositories"
      label="Search"
      filled
      dense
      rounded
      prepend-inner-icon="mdi-magnify"
      solo
    ></v-text-field>
  </form>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      key: process.env.VUE_APP_GITHUB_API_KEY,
      search: "",
      rules: [(v) => v.length <= 256 || "Max 256 characters"],
    };
  },
  methods: {
    submit() {
      console.log(this.search);
      this.$emit("search-text", this.search);
      this.search = "";
    },
  },
};
</script>

<style>
</style>