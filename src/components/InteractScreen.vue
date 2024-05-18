<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardContext"
        :key="index"
        :ref="`card-${index}`"
        :backFaceImageUrl="`images/${card}.png`"
        :card="{ index: index, value: card }"
        @onFlip="checkRule($event)"
        :cardContext="cardContext"
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
    cardContext: {
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
      if (this.rules.length == 2) {
        return false;
      } else {
        this.rules.push(card);
        if (
          this.rules.length == 2 &&
          this.rules[0].value == this.rules[1].value
        ) {
          this.$refs[`card-${this.rules[0].index}`][0].onDisabledMode();
          this.$refs[`card-${this.rules[1].index}`][0].onDisabledMode();
          this.rules = [];
          const disabledElements = document.querySelectorAll(
            ".screen .card.disabled"
          );
          if (
            disabledElements &&
            disabledElements.length == this.cardContext.length - 2
          ) {
            setTimeout(() => {
              this.$emit("onFinish");
            }, 900);
          }
          console.log(disabledElements);
        } else if (
          this.rules.length == 2 &&
          this.rules[0].value !== this.rules[1].value
        ) {
          setTimeout(() => {
            this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
            this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
            this.rules = [];
          }, 800);
        } else return false;
      }
    },
  },
};
</script>
<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}
.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
