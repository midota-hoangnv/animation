<template>
  <div>
    <video loop muted autoplay playsinline width="300" height="300">
      <source :src="CardFinal" />
    </video>
    <div v-if="animationData" class="hello">
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
      <Lottie
        ref="anim"
        :options="defaultOptions"
        :height="96"
        :width="128"
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
    },
    animationCompleted() {
      alert("Loading Completed!");
    },
  },
  data() {
    return {
      animationData: null,
      defaultOptions: {},
      videos: [
        {
          id: 1,
          src: Background,
        },
        // {
        //   id: 2,
        //   src: Character,
        // },
        {
          id: 3,
          src: Halo,
        },
        {
          id: 4,
          src: Thunder,
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
