<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @startGame="handleBeforeStart($event)"
  ></main-screen>
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @endGame="getResult"
  ></interact-screen>

  <result-screen v-if="statusMatch === 'result'" :timer="timer"></result-screen>
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
        total: 0,
        cardsContext: [],
        time: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },

  methods: {
    handleBeforeStart(config) {
      console.log(config);
      this.settings.total = config.total;

      const firstCards = Array.from(
        { length: this.settings.total / 2 },
        (_, i) => i + 1
      );

      const secondCards = [...firstCards];

      const cards = [...firstCards, ...secondCards];

      this.settings.cardsContext = shuffled(cards);

      this.settings.time = new Date().getTime();

      console.log(this.settings.cardsContext);

      this.statusMatch = "match";
    },

    getResult() {
      this.statusMatch = "result";
      this.timer = new Date().getTime() - this.settings.time;
      console.log(this.statusMatch);
    },
  },
};
</script>
