<template>
  <div class="main__content player player--in-progress">
    <h3 class="player__title">
      <span class="player__title-link">{{ src }}</span>
    </h3>
    <div class="player__body">
      <div
        class="player__btn player__btn--play"
        @click="play"
        v-if="playback === false"
      >
        <img src="../assets/icons/play.svg" alt="play" />
      </div>
      <div
        class="player__btn player__btn--play"
        @click="pause"
        v-if="playback === true"
      >
        <img src="../assets/icons/pause.svg" alt="pause" />
      </div>
      <audio class="player__audio" controls ref="audio">
        <source :src="src.replace(/ /g, '')" />
      </audio>
      <div
        class="player__progress"
        @click="setProgress($event)"
        ref="progressBar"
      >
        <div
          class="player__progress-cursor"
          :style="{ width: widthProgress }"
        ></div>
      </div>

      <div class="player__box">
        <div class="player__time">
          {{
            currentTimeMin() < 10 ? "0" + currentTimeMin() : currentTimeMin()
          }}:{{
            currentTimeSec() < 10 ? "0" + currentTimeSec() : currentTimeSec()
          }}
        </div>
        <div class="player__vol" @click="setVolume($event)" ref="volumeBar">
          <div class="player__vol-cursor" :style="{ width: widthVolume }"></div>
        </div>
      </div>
    </div>

    <span
      class="player__back"
      @click="$emit('closePlayer'), (playback = false)"
    >
      <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path
          d="M8 17L3 12M3 12L8 7M3 12H21"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>
      <span class="player__back-txt">Back</span>
    </span>
  </div>
</template>

<script>
export default {
  props: {
    src: String,
  },

  data() {
    return {
      playback: false,
      update: false,
      updateInterval: "",
      keyup: false,
      widthProgress: 0,
      widthVolume: 1,
    };
  },
  mounted() {
    setTimeout(() => {
      this.update = true;
    }, 100);
  },
  updated() {
    this.currentTimeMin();
    this.currentTimeSec();
    this.updateProgress();
    this.volume();
  },
  unmounted() {
    clearInterval(this.updateInterval);
  },
  methods: {
    currentTimeMin() {
      if (this.update) {
        return Math.floor(this.$refs?.audio?.currentTime / 60);
      } else {
        return Math.floor(this.$refs?.audio?.currentTime / 60);
      }
    },
    currentTimeSec() {
      if (this.update) {
        return Math.floor(this.$refs?.audio?.currentTime % 60);
      } else {
        return Math.floor(this.$refs?.audio?.currentTime % 60);
      }
    },
    play() {
      this.updateInterval = setInterval(() => {
        this.update = !this.update;
      }, 1000);
      this.$refs.audio.play();
      this.playback = true;
    },
    pause() {
      this.$refs.audio.pause();
      this.playback = false;
      clearInterval(this.updateInterval);
    },
    updateProgress() {
      const { duration, currentTime, ended } = this.$refs?.audio;
      if (!currentTime) return;
      this.widthProgress = (currentTime / duration) * 100 + "%";
      if (this.widthProgress === "0%") {
        this.widthProgress = "100%";
      }

      if (ended) {
        this.pause();
        this.widthProgress = 0;
      }
    },
    setProgress(e) {
      const widthProgressBar = this.$refs?.progressBar.clientWidth;
      const clickX = e.offsetX;
      const duration = this.$refs?.audio.duration;
      this.$refs.audio.currentTime = (clickX / widthProgressBar) * duration;

      if (!this.playback) {
        this.play();
      }
    },
    volume() {
      this.widthVolume = this.$refs.audio.volume * 100 + "%";
    },
    setVolume(e) {
      this.update = !this.update;
      const widthVolumeBar = this.$refs.volumeBar.clientWidth;
      const clickX = e.offsetX;
      this.$refs.audio.volume = clickX / widthVolumeBar;
    },
  },
};
</script>

<style></style>
