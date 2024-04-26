<template>
  <div class="flex items-center">
    <div class="mr-4">
      <span class="text-xl font-semibold mr-2 timer">{{ formatTime }}</span>
    </div>
    <div>
      <button @click="startTimer" :disabled="timerActive" class="px-4 py-2 bg-blue-500 text-white rounded-md mr-2">Start</button>
      <button @click="stopTimer" :disabled="!timerActive" class="px-4 py-2 bg-red-500 text-white rounded-md mr-2">Stop</button>
      <button @click="resetTimer" class="px-4 py-2 bg-gray-300 text-gray-700 rounded-md">Reset</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      timerActive: false,
      startTime: null,
      elapsedTime: 0
    };
  },
  computed: {
    formatTime() {
      const minutes = Math.floor(this.elapsedTime / 60000);
      const seconds = ((this.elapsedTime % 60000) / 1000).toFixed(0);
      return `${minutes < 10 ? '0' : ''}${minutes}:${+seconds < 10 ? '0' : ''}${seconds}`;
    }
  },
  methods: {
    startTimer() {
      if (!this.timerActive) {
        this.timerActive = true;
        this.startTime = Date.now() - this.elapsedTime;
        this.timer = setInterval(this.updateTimer, 1000);
      }
    },
    stopTimer() {
      if (this.timerActive) {
        this.timerActive = false;
        clearInterval(this.timer);
      }
    },
    resetTimer() {
      this.stopTimer();
      this.elapsedTime = 0;
    },
    updateTimer() {
      this.elapsedTime = Date.now() - this.startTime;
    }
  }
};
</script>
<style>
.timer {
  font-family: "Digital-7"; /* Remplacez "Digital-7" par le nom de votre police numérique */
  font-size: 36px; /* Ajustez la taille de la police selon vos besoins */
  color: #333; /* Couleur du texte */
}
</style>