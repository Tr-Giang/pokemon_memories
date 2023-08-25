<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: setWidth,
        height: setHeight,
      }"
    >
      <card-poke
        v-for="(card, id) in cardsContext"
        :key="id"
        ref="itemRefs"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ id, value: card }"
        @onFlip="checkRules"
        :cardsContext="cardsContext.length"
      ></card-poke>
    </div>
  </div>
</template>

<script>
import CardPoke from "./CardPoke.vue";

export default {
  components: {
    CardPoke,
  },
  props: {
    cardsContext: {
      type: Array,
      required: true,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      rules: [],
      selectedCard: Number,
      widthScreen: window.innerWidth,
      heightScreen: window.innerHeight,
      margin: this.cardsContext.length > 36 ? 6 : 10,
    };
  },
  methods: {
    checkRules(card) {
      console.log(this.margin);
      if (this.rules.length === 2) return false;
      this.rules.push(card);
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        //add class "disabled" to component card
        this.$refs.itemRefs[this.rules[0].id].onEnabledDisableMode();
        this.$refs.itemRefs[this.rules[1].id].onEnabledDisableMode();
        this.rules = [];

        this.selectedCard = document.querySelectorAll(".card.disabled").length;
        if (this.selectedCard === this.cardsContext.length - 2) {
          this.$emit("onFinished");
        }
      }
      if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        //closed

        setTimeout(() => {
          this.$refs.itemRefs[this.rules[0].id].onFlipBackCard();
          this.$refs.itemRefs[this.rules[1].id].onFlipBackCard();

          //reset rules
          this.rules = [];
        }, 800);
      }
    },
  },
  computed: {
    setWidth() {
      if (this.heightScreen <= this.widthScreen) {
        return `${
          ((((this.heightScreen - 64) / Math.sqrt(this.cardsContext.length) -
            this.margin) *
            3) /
            4 +
            this.margin) *
          Math.sqrt(this.cardsContext.length)
        }px`;
      } else {
        return `${
          ((((this.widthScreen - this.margin) / Math.sqrt(this.cardsContext) -
            this.margin) *
            3) /
            4 +
            11) *
          Math.sqrt(this.cardsContext.length)
        }px`;
      }
    },
    setHeight() {
      if (this.heightScreen <= this.widthScreen) {
        return `${this.heightScreen - 64}px`;
      } else {
        return `${this.heightScreen - 64}px`;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.screen__inner {
  margin: 0 auto;
  padding: 1.5vh 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
