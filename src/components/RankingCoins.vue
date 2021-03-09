<template>
  <v-app id="inspire">
    <v-data-table
      :headers="headers"
      :items="coinsRank"
      item-key="coinId"
      hide-default-footer
      disable-filtering
      disable-pagination
      loading="true"
      loading-text="Loading"
      class="elevation-1"
    >
      <template v-slot:item.rankDiff1H="{ item }">
        {{ item.rankPast1H }}
        <v-chip
          v-if="item.rankDiff1H > 0 || item.rankDiff1H < 0"
          :color="getColor(item.rankDiff1H)"
          dark
        >
          {{ rankDiffFormat(item.rankDiff1H) }}
        </v-chip>
      </template>

      <template v-slot:item.rankDiff2H="{ item }">
        {{ item.rankPast2H }}
        <v-chip
          v-if="item.rankDiff2H > 0 || item.rankDiff2H < 0"
          :color="getColor(item.rankDiff2H)"
          dark
        >
          {{ rankDiffFormat(item.rankDiff2H) }}
        </v-chip>
      </template>

      <template v-slot:item.rankDiff4H="{ item }">
        {{ item.rankPast4H }}
        <v-chip
          v-if="item.rankDiff4H > 0 || item.rankDiff4H < 0"
          :color="getColor(item.rankDiff4H)"
          dark
        >
          {{ rankDiffFormat(item.rankDiff4H) }}
        </v-chip>
      </template>

      <template v-slot:item.rankDiff8H="{ item }">
        {{ item.rankPast8H }}
        <v-chip
          v-if="item.rankDiff8H > 0 || item.rankDiff8H < 0"
          :color="getColor(item.rankDiff8H)"
          dark
        >
          {{ rankDiffFormat(item.rankDiff8H) }}
        </v-chip>
      </template>
    </v-data-table>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  data: () => ({
    lastUpdatedAt: "",
    coinsRank: [],
    headers: [
      {
        text: "Coins Name",
        align: "start",
        value: "name",
      },
      { text: "Symbol", value: "symbol" },
      { text: "Current", value: "currentRank" },
      { text: "1H", value: "rankDiff1H" },
      { text: "2H", value: "rankDiff2H" },
      { text: "4H", value: "rankDiff4H" },
      { text: "8H", value: "rankDiff8H" },
    ],
  }),
  methods: {
    getCoinsRank() {
      axios
        .get("https://api.ippo.space/coins-ranking")
        .then((response) => {
          this.lastUpdatedAt = new Date(
            response.data.lastUpdatedAt
          ).toLocaleString();
          this.coinsRank = response.data.coinsRank;
        })
        .catch(function (error) {
          alert(error);
        });
    },
    getColor(rankDiff) {
      if (rankDiff > 0) return "green";
      else if (rankDiff < 0) return "red";
    },
    rankDiffFormat(rankDiff) {
      if (rankDiff < 0) return rankDiff;
      if (rankDiff > 0) return "+".concat(rankDiff);
    },
  },
  mounted() {
    this.getCoinsRank();
  },
};
</script>

<style scoped>
.v-chip {
  font-family: monospace;
  padding: 4px;
  display: unset;
}

.v-chip.v-size--default {
  font-size: 9px;
}
</style>