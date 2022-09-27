<template>
  <div>
    <video loop muted autoplay playsinline width="300" height="300">
      <source :src="CardFinal" />
    </video>
    <div v-if="animationData" class="hello">
      <template v-if="loaded">
        <video
          v-for="video in videos"
          :key="video.id"
          loop
          muted
          autoplay
          playsinline
        >
          <source :src="video.src" />
        </video>
      </template>
      <Lottie
        ref="anim"
        :options="defaultOptions"
        :height="150"
        :width="340"
        class="lottie"
        @DOMLoaded="handleDomloaded"
        @animCreated="handleDomloaded"
      />
    </div>
  </div>
</template>

<script>
import Background from "./../assets/Webm/BG1.webm";
import Character from "./../assets/Webm/Character.webm";
import CardFinal from "./../assets/Webm/CardFinal.mp4";
import Halo from "./../assets/Webm/Halo.webm";
import Thunder from "./../assets/Webm/Thunder.webm";
import TestWebm from "./../assets/Webm/test.webm";
// import TestAvi from "./../assets/Webm/test.265.avi";
import Lottie from "vue-lottie";
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  components: {
    Lottie,
  },
  async created() {
    this.Background = Background;
    this.Character = Character;
    this.CardFinal = CardFinal;
    this.Halo = Halo;
    this.Thunder = Thunder;
    this.TestWebm = TestWebm;
    // this.TestAvi = TestAvi;
    const response = await fetch("/Card.json");
    const animationData = await response.json();
    this.animationData = animationData;
    this.defaultOptions = {
      animationData: animationData,
      autoplay: true,
      loop: true,
    };
  },
  methods: {
    handleDomloaded(anim) {
      console.log(anim);
      this.anim = anim;
      this.anim.addEventListener("complete", () => this.animationCompleted());
      this.anim.addEventListener("DOMLoaded", () => this.imageLoaded());
    },
    animationCompleted() {
      console.log("Loading Completed!");
    },
    imageLoaded() {
      setTimeout(() => {
        this.loaded = true;
      }, 4300);
      console.log("loaded");
    },
  },
  data() {
    return {
      animationData: null,
      defaultOptions: {},
      loaded: false,
      videos: [
        // {
        //   id: 1,
        //   src: Background,
        // },
        // {
        //   id: 2,
        //   src: Character,
        // },
        // {
        //   id: 3,
        //   src: Halo,
        // },
        // {
        //   id: 4,
        //   src: Thunder,
        // },
        // {
        //   id: 5,
        //   src: TestWebm,
        // },
        // {
        //   id: 6,
        //   src: "/images/test.265.avi",
        // },
        {
          id: 7,
          src: CardFinal,
        },
      ],
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.hello {
  background: red;
  width: 500px;
  height: 500px;
  margin: 0 auto;
  position: relative;
  video,
  .lottie {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    margin: 0 !important;
    height: 100%;
    width: 100%;
  }

  .lottie {
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
}
</style>
