<template>
  <main-screen
    v-if="statusMatch == 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMatch == 'match'"
    :cardContext="setting.cardContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMatch == 'result'"
    :timer="timer"
    @onStartAgain="onStartAgain"
  />
  <coppy-right />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import { shuffed } from "./utils/array.js";
import ResultScreen from "./components/ResultScreen.vue";
import CoppyRight from "./components/CoppyRight.vue";
export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CoppyRight,
  },
  data() {
    return {
      setting: {
        totalOfBlocks: 0,
        cardContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      //data ready
      this.setting.totalOfBlocks = config.totalOfBlocks;
      const firstCard = Array.from(
        { length: this.setting.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCard = [...firstCard];
      const cards = [...firstCard, ...secondCard];
      this.setting.cardContext = shuffed(shuffed(shuffed(shuffed(cards))));
      this.setting.startedAt = new Date().getTime();
      this.statusMatch = "match";
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.setting.startedAt;
      this.statusMatch = "result";
    },
    onStartAgain() {
      this.statusMatch = "default";
    },
  },
};
</script>

<style></style>
