<template>
  <div class="main__content main__content--search">
    <h1 class="main__title">Insert the link</h1>
    <form
      class="main__form main__form--error"
      action="#"
      @submit.prevent="$emit('openPlayer')"
    >
      <div class="main__wrapper">
        <label
          class="main__label"
          :class="{
            isValid: !isValidLink,
          }"
        >
          <input
            class="main__input"
            type="text"
            name="link"
            placeholder="https://"
            :value="modelValue"
            @input="updateInput"
          />
        </label>
        <button class="main__btn">
          <svg
            width="40"
            height="36"
            viewBox="0 0 40 36"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fill-rule="evenodd"
              clip-rule="evenodd"
              d="M39.7197 18.6943C40.0934 18.3069 40.0934 17.6931 39.7197 17.3057L23.7197 0.721679L23.0253 0.00201416L21.586 1.39066L22.2803 2.11032L36.6457 17H1H0V19H1H36.6457L22.2803 33.8897L21.586 34.6093L23.0253 35.998L23.7197 35.2783L39.7197 18.6943Z"
              fill="#1B191C"
            />
          </svg>
        </button>
      </div>
      <div class="main__form-error" v-if="!isValidLink">Error message here</div>
    </form>
    <p class="link">
      https://lalalai.s3.us-west-2.amazonaws.com/media/split/a7564eb8-cbf2-40e2-9cb8-
      6061d8d055a7/no_vocals
    </p>
    <p class="link">https://c5.radioboss.fm:18084/stream</p>
  </div>
</template>

<script>
export default {
  props: {
    modelValue: String,
    isValidLink: Boolean,
  },
  methods: {
    updateInput(e) {
      this.$emit("update:modelValue", e.target.value);
    },
  },
};
</script>

<style scoped>
.isValid::after {
  content: "";
  width: 56px;
  height: 56px;
  display: inline-block;
  vertical-align: middle;
  background-image: url("../assets/icons/warning.svg");
  background-position: 50% 50%;
  background-repeat: no-repeat;
  background-size: 24px;
  background-color: #fff;
  position: absolute;
  top: 50%;
  left: auto;
  right: 1px;
  bottom: 0;
  z-index: 2;
  transform: translateY(-50%);
}
</style>
