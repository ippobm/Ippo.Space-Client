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
    >
      <template v-for="h in headers" v-slot:[`item.${h.value}`]="{ item }">
        <div :key="h.value">
          <div v-if="item[h.pastRankPropName]">
            {{ item[h.pastRankPropName] }}
            <v-chip
              v-if="item[h.value] > 0 || item[h.value] < 0"
              :color="getColor(item[h.value])"
              dark
            >
              {{ rankDiffFormat(item[h.value]) }}
            </v-chip>
          </div>
          <div v-else>{{ item[h.value] }}</div>
        </div>
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
    headers: [],
  }),
  methods: {
    getCoinsRanking() {
      axios
        .get("https://api.ippo.space/coins-ranking")
        .then((response) => {
          this.buildCoinsRankArray(response.data.coinsRank);
          this.buildTableHeaders(response.data.coinsRank[0].rankHistory);
        })
        .catch(function (error) {
          alert(error);
        });
    },

    buildCoinsRankArray(coinsRank) {
      coinsRank.forEach((cr) => {
        const coinRank = {
          id: cr.coinId,
          symbol: cr.symbol,
          name: cr.name,
          currentRank: cr.currentRank,
        };

        cr.rankHistory.forEach((rh) => {
          const pastRankPropName = rh.pastHours + "HRank";
          const diffCurrentRankPropName = rh.pastHours + "HDiffCurrentRank";

          coinRank[pastRankPropName] = rh.rank;
          coinRank[diffCurrentRankPropName] = rh.diffCurrentRank;
        });

        this.coinsRank.push(coinRank);
      });
    },

    buildTableHeaders(rankHistory) {
      this.headers.push({ text: "Coins Name", value: "name" });
      this.headers.push({ text: "Symbol", value: "symbol" });
      this.headers.push({ text: "Current", value: "currentRank" });

      rankHistory.forEach((rh) => {
        const diffCurrentRankPropName = rh.pastHours + "HDiffCurrentRank";
        const pastRankPropName = rh.pastHours + "HRank";

        this.headers.push({
          text: rh.pastHours,
          value: diffCurrentRankPropName,
          pastRankPropName: pastRankPropName,
        });
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
    this.getCoinsRanking();
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