<template>
  <div class="countdown">
    <div class="timer-container">
      <div class="timer" :style="{ color: countdownColor }">{{ formatTime }}</div>
    </div>
    <div class="controls">
      <input type="number" v-model="countdownDuration" min="1" class="input-field">
      <button @click="startCountdown" :disabled="countdownActive" class="btn start">Démarrer</button>
      <button @click="stopCountdown" :disabled="!countdownActive" class="btn stop">Arrêter</button>
      <button @click="resetCountdown" class="btn reset">Réinitialiser</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      countdownActive: false,
      countdownDuration: 60,
      countdownColor: '#333'
    };
  },
  computed: {
    formatTime() {
      const minutes = Math.floor(this.countdownDuration / 60);
      const seconds = this.countdownDuration % 60;
      return `${minutes < 10 ? "0" : ""}${minutes}:${seconds < 10 ? "0" : ""}${seconds}`;
    }
  },
  methods: {
    startCountdown() {
      if (!this.countdownActive && this.countdownDuration > 0) {
        this.countdownActive = true;
        this.countdownTimer = setInterval(this.updateCountdown, 1000);
      }
    },
    stopCountdown() {
      if (this.countdownActive) {
        this.countdownActive = false;
        clearInterval(this.countdownTimer);
      }
    },
    resetCountdown() {
      this.stopCountdown();
      this.countdownDuration = 60;
      this.countdownColor = '#333';
    },
    updateCountdown() {
      if (this.countdownDuration > 0) {
        this.countdownDuration--;
      } else {
        this.stopCountdown();
      }
      const hue = (this.countdownDuration * 6) % 360;
      this.countdownColor = `hsl(${hue}, 100%, 50%)`;
    }
  }
};
</script>

<style scoped>
.countdown {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.timer-container {
  margin-bottom: 1rem;
}

.timer {
  font-size: 2rem;
}

.controls {
  display: flex;
  align-items: center;
}

.input-field {
  width: 5rem;
  padding: 0.5rem;
  margin-right: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 0.25rem;
}

.btn {
  padding: 0.5rem 1rem;
  margin-right: 0.5rem;
  border: none;
  border-radius: 0.25rem;
  cursor: pointer;
}

.btn.start {
  background-color: #4caf50;
  color: white;
}

.btn.stop {
  background-color: #f44336;
  color: white;
}

.btn.reset {
  background-color: #e0e0e0;
  color: #333;
}
</style>
