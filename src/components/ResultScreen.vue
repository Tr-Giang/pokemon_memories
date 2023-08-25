<template>
  <div class="screen">
    <h1>✨ Congratulations ✨</h1>
    <p>{{ timer }} Seconds</p>
    <base-button @click="playAgain">Play again</base-button>
  </div>
</template>

<script>
import BaseButton from "./base/BaseButton.vue";
export default {
  components: {
    BaseButton,
  },
  methods: {
    playAgain() {
      this.$emit("playAgain");
    },
  },
  props: {
    timer: {
      type: Number,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped>
.screen {
  justify-content: center;
  p {
    margin: 1rem 0;
  }
}
</style>
