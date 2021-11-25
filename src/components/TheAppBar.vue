<template>
  <v-app-bar app color="primary" dark>
    <div class="d-flex align-center">
      <div class="text-h5">{{ $t("system.title") }}</div>
    </div>

    <v-spacer></v-spacer>

    <v-btn text @click="changeLang">
      <v-icon>mdi-translate</v-icon>
    </v-btn>
    <v-btn href="https://github.com/monoglo/card-creator" target="_blank" text>
      <span class="mr-2">github</span>
      <v-icon>mdi-github</v-icon>
    </v-btn>
  </v-app-bar>
</template>

<script>
export default {
  methods: {
    changeLang() {
      if (this.$i18n.locale === "zh") {
        this.$i18n.locale = "en";
      } else {
        this.$i18n.locale = "zh";
      }
    },
  },
};
</script>

<style></style>
