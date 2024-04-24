<template>
  <div class="clock">
    <div v-if="clockStyle === 'modern'" class="digital-clock">
      {{ currentTime.toLocaleTimeString() }}
    </div>
    <svg v-else :viewBox="viewBox" class="clock-face">
      <circle class="clock-circle" cx="50%" cy="50%" :r="radius" />
      <g v-for="(number, index) in 12" :key="index">
        <text
          :x="getTextX(index)"
          :y="getTextY(index)"
          :font-size="getFontSize()"
          :text-anchor="getTextAnchor(index)"
        >
          {{ number }}
        </text>
      </g>
      <line
        class="hour-hand"
        :x1="centerX"
        :y1="centerY"
        :x2="hourX"
        :y2="hourY"
      />
      <line
        class="minute-hand"
        :x1="centerX"
        :y1="centerY"
        :x2="minuteX"
        :y2="minuteY"
      />
      <line
        class="second-hand"
        :x1="centerX"
        :y1="centerY"
        :x2="secondX"
        :y2="secondY"
      />
      <circle class="center-circle" cx="50%" cy="50%" r="3" />
    </svg>
    <div class="options mt-4">
      <label for="timezone" class="mr-2">Fuseau horaire :</label>
      <select
        v-model="timezone"
        id="timezone"
        class="border border-gray-300 rounded-md py-2 px-4"
        @change="updateClock"
      >
        <option value="auto">Auto</option>
        <option value="gmt">GMT</option>
        <option value="est">EST</option>
        <option value="pst">PST</option>
      </select>
      <label for="clock-style" class="ml-4 mr-2">Style de l'horloge :</label>
      <select
        v-model="clockStyle"
        id="clock-style"
        class="border border-gray-300 rounded-md py-2 px-4"
        @change="updateClock"
      >
        <option value="standard">Standard</option>
        <option value="modern">Moderne</option>
      </select>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      timezone: "auto",
      clockStyle: "standard",
      currentTime: new Date(),
    };
  },
  computed: {
    viewBox() {
      return `0 0 ${this.radius * 2} ${this.radius * 2}`;
    },
    radius() {
      return 100;
    },
    centerX() {
      return this.radius;
    },
    centerY() {
      return this.radius;
    },
    // Pour le calcul de la position de l'aiguille des heures
    hourX() {
      const hourAngle =
        (((this.currentTime.getHours() % 12) * 30 +
          this.currentTime.getMinutes() / 2) *
          Math.PI) /
        180;
      return this.centerX + Math.sin(hourAngle) * 40;
    },
    hourY() {
      const hourAngle =
        (((this.currentTime.getHours() % 12) * 30 +
          this.currentTime.getMinutes() / 2) *
          Math.PI) /
        180;
      return this.centerY - Math.cos(hourAngle) * 40;
    },

    // Pour le calcul de la position de l'aiguille des minutes
    minuteX() {
      const minuteAngle = (this.currentTime.getMinutes() * 6 * Math.PI) / 180;
      return this.centerX + Math.sin(minuteAngle) * 50;
    },
    minuteY() {
      const minuteAngle = (this.currentTime.getMinutes() * 6 * Math.PI) / 180;
      return this.centerY - Math.cos(minuteAngle) * 50;
    },

    // Pour le calcul de la position de l'aiguille des secondes
    secondX() {
      const secondAngle = (this.currentTime.getSeconds() * 6 * Math.PI) / 180;
      return this.centerX + Math.sin(secondAngle) * 60;
    },
    secondY() {
      const secondAngle = (this.currentTime.getSeconds() * 6 * Math.PI) / 180;
      return this.centerY - Math.cos(secondAngle) * 60;
    },
  },
  methods: {
    updateClock() {
      setInterval(() => {
        this.currentTime = new Date();
      }, 1000);
    },

    getTextX(index) {
      const angle = ((index + 1) * 30 * Math.PI) / 180;
      return this.centerX + Math.sin(angle) * 70;
    },
    getTextY(index) {
      const angle = ((index + 1) * 30 * Math.PI) / 180;
      return this.centerY - Math.cos(angle) * 70 + 5; // Ajustement pour centrer le texte verticalement
    },
    getFontSize() {
      return 14; // Taille de police des chiffres
    },
    getTextAnchor(index) {
      return index === 0 ? "middle" : index < 6 ? "start" : "end";
    },
  },
  mounted() {
    this.updateClock();
  },
};
</script>

<style scoped>
.clock {
  display: inline-block;
  text-align: center;
}

.clock-face {
  width: 200px;
  height: 200px;
}

.clock-circle {
  fill: none;
  stroke: black;
  stroke-width: 2;
}

.hour-hand {
  stroke: black;
  stroke-width: 6;
}

.minute-hand {
  stroke: black;
  stroke-width: 4;
}

.second-hand {
  stroke: red;
  stroke-width: 2;
}

.center-circle {
  fill: black;
}

.digital-clock {
  font-size: 24px;
  margin-top: 20px;
}
</style>
