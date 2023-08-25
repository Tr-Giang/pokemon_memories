<template>
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${setHeight}px`,
      width: `${setWidth}px`,
      margin: `${margin / 2}px`,
      perspective: `${setWidth * 2}px`,
    }"
  >
    <div
      class="card__inner"
      :class="{ 'is-flipped': isFlipped }"
      @click="onToggleFlip"
    >
      <div class="card__face card__face--front">
        <div
          class="card-content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`,
          }"
        ></div>
      </div>
      <div class="card__face card__face--back">
        <div class="card-content"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isFlipped: false,
      isDisabled: false,
      heightScreen: window.innerHeight,
      widthScreen: window.innerWidth,
      margin: this.cardsContext > 36 ? 6 : 10,
    };
  },
  methods: {
    onToggleFlip() {
      if (this.isDisabled) return false;
      this.isFlipped = !this.isFlipped;

      if (this.isFlipped === true) {
        this.$emit("onFlip", this.card);
      }
    },
    onFlipBackCard() {
      this.isFlipped = false;
    },
    onEnabledDisableMode() {
      this.isDisabled = true;
    },
  },
  props: {
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
    card: {
      type: [String, Number, Array, Object],
    },
    cardsContext: {
      type: Number,
      required: true,
    },
  },
  computed: {
    setHeight() {
      if (this.heightScreen <= this.widthScreen) {
        return (
          (this.heightScreen - 64) / Math.sqrt(this.cardsContext) - this.margin
        );
      } else {
        return (
          (((this.widthScreen - this.margin) / Math.sqrt(this.cardsContext) -
            this.margin) *
            4) /
          3
        );
      }
    },
    setWidth() {
      if (this.heightScreen <= this.widthScreen) {
        return (
          (((this.heightScreen - 64) / Math.sqrt(this.cardsContext) -
            this.margin) *
            3) /
          4
        );
      } else {
        return (
          (this.widthScreen - this.margin) / Math.sqrt(this.cardsContext) -
          this.margin
        );
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.card {
  display: inline-block;

  .card__inner {
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;

    &.is-flipped {
      transform: rotateY(-180deg);
    }

    .card__face {
      position: absolute;
      width: 100%;
      height: 100%;
      backface-visibility: hidden;
      overflow: hidden;
      border-radius: 10%;
      box-shadow: 0 3px 10px 3px rgba($color: #000000, $alpha: 0.5);
      &--front {
        background-color: var(--light);
        transform: rotateY(-180deg);
        padding: 10%;

        .card-content {
          background-position: center center;
          background-repeat: no-repeat;
          background-size: contain;
          height: 100%;
          width: 100%;
        }
      }
      &--back .card-content {
        background: url("../assets/images/icon_back.png") no-repeat center
          center;
        background-size: contain;
        width: 100%;
        height: 100%;
      }
      &--back {
        background: var(--dark);
        padding: 25%;
      }
    }
  }

  &.disabled {
    .card__inner {
      cursor: default;
    }
  }
}
</style>
