<template>
  <div v-show="show" class="animation-wrap">
    <div ref="animation" class="animation-content"></div>
    <slot
      v-if="initLottieDone"
      :frames="element.totalFrames"
      :onEnterFrame="onEnterFrame"
      :showIntro="showIntro"
      name="circle"
    ></slot>
    <slot name="skip"></slot>
    <slot name="finished"></slot>
  </div>
</template>

<script>
import lottie from "lottie-web";

export default {
  props: {
    path: {
      type: String,
    },
    autoplay: {
      type: Boolean,
      default: true,
    },
    loop: {
      type: Boolean,
      default: false,
    },
    callEvent: {
      type: Function,
      default: () => {},
    },
    animationData: {
      type: Object,
      required: true,
    },
    show: {
      type: Boolean,
    },
  },
  data() {
    return {
      element: null,
      initLottieDone: false,
      showIntro: true,
    };
  },
  mounted() {
    this.initLottie();
  },
  beforeDestroy() {
    this.element.destroy();
  },
  watch: {
    show(show) {
      if (show && this.autoplay) {
        this.element.play();
      }
    },
  },
  methods: {
    async initLottie() {
      this.element = lottie.loadAnimation({
        container: this.$refs.animation,
        renderer: "svg",
        loop: this.loop,
        autoplay: false,
        animationData: this.animationData,
      });
      this.registerEvent();
      // fetch(this.path)
      //   .then(response => response.json())
      //   .then(animationData => {
      //     if (animationData == undefined || animationData == null) {
      //       this.$emit("onError");
      //     } else {
      //       this.element = lottie.loadAnimation({
      //         container: this.$el,
      //         renderer: "svg",
      //         loop: this.loop,
      //         autoplay: this.autoplay,
      //         animationData
      //       });
      //       this.registerEvent();
      //     }
      //   })
      //   .catch(errors => {
      //     this.$emit("onError");
      //     handleError(errors);
      //   });
    },
    registerEvent() {
      this.element.addEventListener("complete", (event) => {
        this.$emit("onComplete", event);
      });
      this.element.addEventListener("DOMLoaded", () => {
        this.callEvent(this);
      });

      this.element.addEventListener("loaded_images", () => {
        this.$emit("ready");
        this.initLottieDone = true;
      });
    },
    play() {
      this.element.play();
    },
    onEnterFrame(frame) {
      this.showIntro = false;
      this.element.setCurrentRawFrameValue(frame);
    },
  },
};
</script>

<style lang="scss" scoped>
.animation-content {
  height: 100%;
  width: 100%;
}

.animation-wrap {
  height: 100%;
  width: 100%;
  position: absolute;
  top: 0;
}
</style>
