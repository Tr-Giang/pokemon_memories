<template>
  <div class="screen">
    <h1>POKE MEMORIES</h1>
    <h3>Select mode to start game:</h3>
    <div class="actions">
      <base-button @click="onStart(16)">
        <spa class="number">4x4</spa>
        <span>Easy</span>
      </base-button>
      <base-button @click="onStart(36)">
        <spa class="number">6x6</spa>
        <span>Normal</span>
      </base-button>
      <base-button @click="onStart(64)">
        <spa class="number">8x8</spa>
        <span>Hard</span>
      </base-button>
      <base-button @click="onStart(100)">
        <span class="number">10x10</span>
        <span>Supper Hard</span>
      </base-button>
    </div>
  </div>
</template>

<script>
import BaseButton from "./base/BaseButton.vue";
export default {
  methods: {
    onStart(num) {
      this.$emit("onStart", { totalOfBlocks: num });
    },
  },
  components: {
    BaseButton,
  },
};
</script>

<style lang="scss" scoped>
.screen {
  justify-content: center;
  span {
    display: block;
  }

  .number {
    font-size: 20px;
  }

  .actions {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    margin-top: 1.5rem;
  }
}
</style>
