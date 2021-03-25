<template>
  <v-app dark>
    <AppHeader />
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import AppHeader from '../components/AppHeader.vue'
export default {
  components: { AppHeader },
}
</script>
