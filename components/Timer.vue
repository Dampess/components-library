<template>
  <div class="flex flex-col items-center">
    <div class="mr-4 mb-4 circle">
      <span :style="{ color: timerColor }" class="text-xl font-semibold mr-2 timer">{{ formatTime }}</span>
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
      elapsedTime: 0,
      timerColor: '#333' // Initial color
    };
  },
  computed: {
    formatTime() {
      const minutes = Math.floor(this.elapsedTime / 60000);
      const seconds = ((this.elapsedTime % 60000) / 1000).toFixed(0);
      return `${minutes < 10 ? "0" : ""}${minutes}:${+seconds < 10 ? "0" : ""}${seconds}`;
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
      this.timerColor = '#333'; // Reset color
    },
    updateTimer() {
      this.elapsedTime = Date.now() - this.startTime;
      // Modify color based on elapsed time (for demonstration purposes)
      const hue = (this.elapsedTime / 1000) * 0.36 % 360; // Change color every 10 seconds
      this.timerColor = `hsl(${hue}, 100%, 50%)`;
    }
  }
};
</script>

<style>
.timer {
  font-family: "Digital-7"; /* Remplacez "Digital-7" par le nom de votre police numérique */
  font-size: 40px; /* Ajustez la taille de la police selon vos besoins */
  transition: color 0.5s ease; /* Animation de transition de couleur */
}
.circle {
  border: 5px dotted #333;
  border-radius: 50%;
  padding: 5%;
  padding-top: 7%;
  padding-bottom: 7%;
}
</style>
