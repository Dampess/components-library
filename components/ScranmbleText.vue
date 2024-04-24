<template>
  <div class="relative inline-block text-black bg-red-600 text-center text-4xl">
    <span :class="{ 'scrolling': isScrolling }">{{ currentText }}</span>
  </div>
</template>

<script>
export default {
  props: {
    texts: {
      type: Array,
      required: true
    },
    interval: {
      type: Number,
      default: 3000
    }
  },
  data() {
    return {
      textIndex: 0,
      timer: null,
      isScrolling: false
    };
  },
  computed: {
    currentText() {
      return this.texts[this.textIndex];
    }
  },
  mounted() {
    this.timer = setInterval(this.changeText, this.interval);
  },
  beforeDestroy() {
    clearInterval(this.timer);
  },
  methods: {
    changeText() {
      this.isScrolling = true;
      this.textIndex = (this.textIndex + 1) % this.texts.length;
      setTimeout(() => {
        this.isScrolling = false;
      }, 1000); // Reset isScrolling after 1 second
    }
  }
};
</script>

<style>
.scrolling {
  animation: scrollAnimation 1s linear;
}

@keyframes scrollAnimation {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(-100%);
  }
}
</style>
