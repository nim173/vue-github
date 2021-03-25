<template>
  <v-card
    class="mx-auto my-4"
    color="#24292e"
    dark
    max-width="600"
    min-width="50"
  >
    <v-card-title>
      <v-icon medium left> mdi-github </v-icon>
      <nuxt-link :to="repo.full_name">
        <span class="title font-weight-regular">{{ repo.full_name }}</span>
      </nuxt-link>
    </v-card-title>

    <v-card-text class="font-weight-bold" v-if="repo.description">
      {{ repo.description }}
    </v-card-text>

    <v-card-actions>
      <v-list-item class="grow">
        <v-list-item-avatar color="grey darken-3">
          <v-img
            class="elevation-6"
            alt=""
            :src="repo.owner.avatar_url"
          ></v-img>
        </v-list-item-avatar>

        <v-list-item-content>
          <nuxt-link :to="{ path: '/' + repo.owner.login }">
            <v-list-item-title>{{ repo.owner.login }}</v-list-item-title>
          </nuxt-link>
        </v-list-item-content>

        <v-row align="center" justify="end">
          <v-icon class="mr-1"> mdi-star </v-icon>
          <span class="subheading mr-2">{{ repo.stargazers_count }}</span>
          <span class="mr-1">·</span>
          <v-icon class="mr-1"> mdi-source-fork </v-icon>
          <span class="subheading">{{ repo.forks_count }}</span>
        </v-row>
      </v-list-item>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  name: "Repo",
  props: {
    repo: {
      id: Number,
      full_name: String,
      description: String,
      stargazers_count: Number,
      forks_count: Number,
      owner: {
        login: String,
        avatar_url: String,
        html_url: String,
      },
    },
  },
};
</script>

<style>
</style>