<template>
  <main-screen
    @onStart="onHandleBeforeStart($event)"
    v-if="statusMatch == 'default'"
  />
  <interact-screen
    v-if="statusMatch == 'match'"
    :cardsContext="settings.cardContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMatch == 'result'"
    :timer="timer"
    @onStartAgain="statusMatch == 'default'"
  />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
// import CopyRightScreen from "./components/CopyRightScreen.vue";
import { shuffled } from "./utils/array";
export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    // CopyRightScreen,
  },
  data() {
    return {
      settings: {
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
      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];

      this.settings.cardContext = shuffled(shuffled(shuffled(shuffled(cards))));

      this.settings.startedAt = new Date().getTime();

      this.statusMatch = "match";
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startedAt;

      this.statusMatch = "result";
    },
  },
};
</script>
