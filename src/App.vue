<template>
  <v-app>
    <v-app-bar color="deep-orange" dark>
      <v-toolbar-title> Ippo.Space 👨‍🚀🚀🌝</v-toolbar-title>
    </v-app-bar>
    <v-main>
      <RankingCoins />
    </v-main>

    <v-footer class="font-weight-small" color="deep-orange" dark>
      <v-col class="text-center">
        {{ new Date().getFullYear() }} — <strong>Ippo.Space</strong>
      </v-col>
    </v-footer>
  </v-app>
</template>

<script>
import RankingCoins from "./components/RankingCoins";

export default {
  name: "App",
  components: {
    RankingCoins,
  },
};
</script>