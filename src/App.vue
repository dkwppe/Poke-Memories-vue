<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <!-- so luong card ma nguoi choi chon-->
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import { shuffled } from "./utils/array";
import ResultScreen from "./components/ResultScreen.vue";
export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
  },
  data() {
    return {
      settings: {
        totalOfBlock: 0,
        cardsContext: [], //chua cac gia tri tu 1->8 da mix
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },

  methods: {
    onHandleBeforeStart(configs) {
      console.log("running handle before start, ", configs);
      this.settings.totalOfBlocks = configs.totalOfBlocks;

      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      console.log(cards);
      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
      console.log(this.settings.cardsContext);
      console.log(firstCards);
      this.settings.startedAt = new Date().getTime();
      //data ready
      this.statusMatch = "match"; //trc khi bat dau game
    },
    onGetResult() {
      // ket qua thoi gian
      this.timer = new Date().getTime() - this.settings.startedAt;

      //switch to result component
      this.statusMatch = "result";
    },
  },
};
</script>
