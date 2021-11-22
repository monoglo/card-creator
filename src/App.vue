<template>
  <v-app>
    <the-app-bar></the-app-bar>
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
import TheAppBar from "./components/TheAppBar.vue";
export default {
  name: "App",
  components: {
    "the-app-bar": TheAppBar,
  },
  data: () => ({
    //
  }),
};
</script>
