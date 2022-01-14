<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  ></main-screen>
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult()"
  ></interact-screen>
  <result-screen
    :timer="timer"
    v-if="statusMatch === 'result'"
    @onStartAgain="onStartAgain"
  ></result-screen>
  <copy-right></copy-right>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import { shuffled } from "./utils/array";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRight from "./components/CopyRight.vue";

export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },

  methods: {
    onHandleBeforeStart(config) {
      console.log("running handle before start!");
      this.settings.totalOfBlocks = config.totalOfBlocks;
      const firstCards = Array.from(
        {
          length: this.settings.totalOfBlocks / 2,
        },
        (_, i) => i + 1
      );

      const cards = [...firstCards, ...firstCards];

      this.settings.cardsContext = shuffled(shuffled(shuffled(cards)));

      this.settings.startedAt = new Date().getTime();
      //data ready
      this.statusMatch = "match";
    },
    onGetResult() {
      //get Timer
      this.timer = new Date().getTime() - this.settings.startedAt;
      //switch to result component
      console.log(this.statusMatch);
      this.statusMatch = "result";
      console.log(this.statusMatch);
    },
    onStartAgain() {
      this.statusMatch = "default";
    },
  },
};
</script>
