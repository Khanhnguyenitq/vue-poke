<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 64) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 + 16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index: index, value: card }"
        :cardsContext="cardsContext"
        @onFlip="checkRule($event)"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./CardFlip.vue";

export default {
  data() {
    return {
      rules: [],
    };
  },
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip,
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;
      this.rules.push(card);

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        console.log("right");
        this.$refs[`card-${this.rules[0].index}`][0].flipMode();
        this.$refs[`card-${this.rules[1].index}`][0].flipMode();

        this.rules = [];

        const disabledElements = document.querySelectorAll(".disabled");
        console.log(disabledElements);

        if (
          disabledElements &&
          disabledElements.length === this.cardsContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("endGame");
          }, 900);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        console.log("wrong");
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].flipBack();
          this.$refs[`card-${this.rules[1].index}`][0].flipBack();

          this.rules = [];
        }, 800);
      } else {
        return false;
      }
      console.log(this.rules);
    },
  },
};
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}

.screen__inner {
  display: block;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
