<template>
  <v-app>
    <v-app-bar color="deep-orange" dark>
      <v-toolbar-title>Ippo.Space</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn icon>
        <!-- <v-icon>mdi-magnify</v-icon> -->
      </v-btn>
    </v-app-bar>
    <v-main>
      <RankingCoins />
    </v-main>
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