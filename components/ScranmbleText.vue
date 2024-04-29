<template>
  <div class="relative rounded p-2 inline-block text-black bg-gray-600 text-center text-4xl">
    <span :class="{ 'scrolling': isScrolling }">{{ displayText }}</span>
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
      isScrolling: false,
      transitionText: '',
      transitionTimeout: null,
      letterIndex: 0,
      specialChars: ['!', '@', '#', '$', '%', '^', '&', '*']
    };
  },
  computed: {
    displayText() {
      return this.transitionText || this.texts[this.textIndex];
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
      const nextText = this.texts[(this.textIndex + 1) % this.texts.length];
      this.transitionText = '';
      this.isScrolling = true;
      this.letterIndex = 0;
      this.transitionTimeout = setInterval(() => {
        if (this.letterIndex < nextText.length) {
          // Alterne entre les caractères spéciaux et les lettres originales
          if (this.letterIndex % 2 === 0) {
            this.transitionText += nextText.charAt(this.letterIndex);
          } else {
            this.transitionText += this.specialChars[Math.floor(Math.random() * this.specialChars.length)];
          }
          this.letterIndex++;
        } else {
          clearInterval(this.transitionTimeout);
          this.textIndex = (this.textIndex + 1) % this.texts.length;
          setTimeout(() => {
            this.isScrolling = false;
          }, 5000); // Augmente le délai de réinitialisation de l'animation
        }
      }, 100); // Ralentit la vitesse de changement de lettre
    }
  }
};
</script>

<style scoped>
.scrolling {
  animation: scrollDown 1s linear;
}

@keyframes scrollDown {
  from {
    transform: translateY(-100%);
  }
  to {
    transform: translateY(0);
  }
}
</style>
