<template>
  <div>
    <div v-if="isFinish" class="hello">
      <template>
        <video loop muted autoplay playsinline>
          <source :src="Background" />
        </video>
      </template>
      <Lottie
        v-for="lottie in lotties"
        :key="lottie.id"
        :options="lottie.defaultOptions"
        :height="150"
        :width="340"
        class="lottie"
        @animCreated="handleDomloaded"
      />
      <template>
        <video
          style="opacity: 0"
          id="greenscreenvideo"
          :src="Thunder"
          loop
          autoplay
          muted
        ></video>
        <a-scene embedded vr-mode-ui="false">
          <a-plane
            material="shader: chromakey; src: #greenscreenvideo; color: 0.05098039215 0.28235294117 0.01176470588"
            position="0 1 -10"
            scale="15 25 15"
          ></a-plane>
        </a-scene>
      </template>
      <!-- <template>
        <video
          style="opacity: 0"
          id="greenscreenvideo"
          :src="Particule"
          loop
          autoplay
          muted
        ></video>
        <a-scene embedded vr-mode-ui="false">
          <a-plane
            material="shader: chromakey; src: #greenscreenvideo; color: 0.913725490196078 0.043137254901961 1"
            position="0 1 -10"
            scale="15 25 15"
          ></a-plane>
        </a-scene>
      </template> -->
    </div>
  </div>
</template>

<script>
import Background from "./../assets/Webm/GachaAnimation/BackGround1.mp4";
import Thunder from "./../assets/Webm/GachaAnimation/Thunder4/Thunder.mp4";
import Particule from "./../assets/Webm/particule.mp4";
import Lottie from "vue-lottie";
import "aframe";
import "aframe-chromakey-material";
export default {
  name: "DemoAnimation",
  props: {
    msg: String,
  },
  components: {
    Lottie,
  },
  async created() {
    this.Background = Background;
    this.Thunder = Thunder;
    this.Particule = Particule;
    await this.fetchLotties();
  },
  methods: {
    async fetchLotties() {
      for (let i = 0; i < this.lotties.length; i++) {
        const response = await fetch(this.lotties[i].src);
        const animationData = await response.json();
        this.lotties[i].animationData = animationData;
        this.lotties[i].defaultOptions = {
          animationData: animationData,
          autoplay: true,
          loop: true,
        };
      }
      this.$forceUpdate();
      this.isFinish = true;
      console.log("Data", this.lotties);
    },
    handleDomloaded(anim) {
      console.log("handleDOm", anim);
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
      loaded: false,
      lotties: [
        {
          id: 1,
          src: "/JsonPerson.json",
          animationData: null,
          defaultOptions: {},
        },
        {
          id: 2,
          src: "/girls.json",
          animationData: null,
          defaultOptions: {},
        },
        {
          id: 3,
          src: "/Card.json",
          animationData: null,
          defaultOptions: {},
        },
      ],
      isFinish: false,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.hello {
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
