<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  ></main-screen>
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinished="onFinished"
  ></interact-screen>
  <result-screen
    v-if="statusMatch === 'result'"
    @playAgain="playAgain"
    :timer="timer"
  ></result-screen>
  <copy-right-screen></copy-right-screen>
</template>

<script>
import InteractScreen from "./components/InteractScreen.vue";
import MainScreen from "./components/MainScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRightScreen from "./components/CopyRightScreen.vue";
import { shuffled } from "./utils/array";
export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRightScreen,
  },
  data() {
    return {
      statusMatch: "default",
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      timer: null,
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondedCards = [...firstCards];

      const cards = [...secondedCards, ...firstCards];

      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
      this.settings.startedAt = new Date().getTime();
      //data ready
      this.statusMatch = "match";
    },
    onFinished() {
      setTimeout(() => {
        this.statusMatch = "result";
      }, 1000);

      this.timer = Math.round(
        (new Date().getTime() - this.settings.startedAt) / 1000
      );
    },
    playAgain() {
      this.statusMatch = "default";
    },
  },
};
</script>
