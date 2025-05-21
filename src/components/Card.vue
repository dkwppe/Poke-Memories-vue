<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <!-- Thẻ bao ngoài cho card -->
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${(920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px`,
      width: `${
        (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4
      }px`,
      perspective: `${
        ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4) * 2
      }px`,
    }"
  >
    <!-- Thẻ bên trong có hiệu ứng lật -->
    <div
      class="card_inner"
      :class="{ 'is-flipped': isFlipped }"
      @click="onToggleFlipCard"
    >
      <!-- Mặt trước của thẻ -->
      <div class="card_face card_face--front">
        <div
          class="card_content"
          :style="{
            backgroundSize: `${
              (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <!-- Mặt sau của thẻ -->
      <div class="card_face card_face--back">
        <div
          class="card_content"
          :style="{
            backgroundImage: `url('${require('@/assets/' + imgBackFaceUrl)}')`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    card: {
      type: [String, Number, Array, Object],
    },
    imgBackFaceUrl: {
      type: String,
      require: true,
    },
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isDisabled: false,
      //bien kiem soat trang thai lat cua the
      isFlipped: false,
    };
  },
  methods: {
    // Hàm xử lý khi người dùng click vào thẻ
    onToggleFlipCard() {
      if (this.isDisabled) return false;
      //Đảo ngược trạng thái lật (true <=> false)
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) this.$emit("onFlip", this.card);
    },

    onFlipBackCard() {
      this.isFlipped = false;
    },
    onEnabledDisableMode() {
      this.isDisabled = true;
    },
  },
};
</script>

<style lang="css" scoped>
/* the chinh */
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}
.card_inner {
  width: 100%;
  height: 100%;
  transition: transform 1s; /*lat trong 1s */
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}
.card_inner.is-flipped {
  transform: rotateY(-180deg);
}
.card_face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.3);
}
.card_face--front .card_content {
  background: url("../assets/image/icon_back.png") no-repeat center center;
  height: 100%;
  width: 100%;
}

/* cho mat sau len truoc*/
.card_face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}
.card_face--back .card_content {
  background-size: contain;
  background-position: center center;
  background-repeat: no-repeat;
  height: 100%;
  width: 100%;
}
.card.disabled .card_inner {
  cursor: default;
}
</style>
