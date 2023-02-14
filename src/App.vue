<script>
import AddLink from "./components/AddLink.vue";
import AudioPlayer from "./components/AudioPlayer.vue";
export default {
  components: {
    AddLink,
    AudioPlayer,
  },
  data() {
    return {
      isValidLink: true,
      src: "",
      openPlayer: false,
    };
  },

  methods: {
    openPlayerFn() {
      if (this.src.length !== 0 && this.isValidLink === true) {
        this.openPlayer = true;
      } else {
        return;
      }
    },
  },
  watch: {
    src() {
      if (!this.src.startsWith("https://")) {
        this.isValidLink = false;
      } else {
        this.isValidLink = true;
      }
      if (this.src.length === 0) {
        this.isValidLink = true;
      }
    },
  },
};
</script>

<template>
  <div class="main">
    <div class="main__inner">
      <div class="container">
        <add-link
          v-if="!openPlayer"
          v-model.trim="src"
          :modelValue="$event"
          :isValidLink="isValidLink"
          @openPlayer="openPlayerFn"
        ></add-link>
        <audio-player
          v-if="openPlayer"
          :src="src"
          @closePlayer="openPlayer = false"
        ></audio-player>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
