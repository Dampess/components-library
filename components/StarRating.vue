<template>
  <div class="star-rating" @mouseleave="resetHover">
    <div v-for="index in maxStars" :key="index" class="star">
      <span
        :class="{ filled: index <= currentRating }"
        @mouseover="hoverOver(index)"
        @click="selectRating(index)"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
          width="24"
          height="24"
          :class="starColor(index),hoverStarColor(index)"
        >
          <path
            d="M12 2L9.45 8.09 2 9.24l5.46 4.73L5.82 21 12 17.27 18.18 21l-1.63-7.03L22 9.24l-7.45-1.15z"
          
            
          />
        </svg>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    maxStars: {
      type: Number,
      default: 5, // Nombre maximal d'étoiles
    },
    interactive: {
      type: Boolean,
      default: true, // Indicateur de notation interactive
    },
    initialRating: {
      type: Number,
      default: 2, // Note initiale
    },
    //activeColor: {
    //  type: String,
    // default: "#ffcc00" // Couleur des étoiles sélectionnées
    // },
    //inactiveColor: {
    // type: String,
    //default: "#ccc", // Couleur des étoiles non sélectionnées
    // },
  },
  data() {
    return {
      currentRating: this.initialRating,
      hoverRating: 0,
    };
  },
  methods: {
    starColor(index) {
      let color = index <= this.currentRating ? "activeColor" : "inactiveColor";
      return color;
    },
    hoverStarColor(index) {
      console.log('entrée')
      console.log(this.hoverRating)
      console.log(index)
      let color = index <= this.hoverRating ? 'activeColor' : 'inactiveColor';
      return color
    },
    hoverOver(index) {
      if (this.interactive) {
        this.hoverRating = index;
      }
    },
    resetHover() {
      if (this.interactive) {
        this.hoverRating = 0;
      }
    },
    selectRating(index) {
      if (this.interactive) {
        this.currentRating = index;
        this.$emit("rating-selected", this.currentRating);
      }
    },
  },
};
</script>

<style scoped>
.activeColor {
  fill: #ffcc00;
}
.inactiveColor {
  fill: #ccc;
}
.star-rating {
  font-size: 24px; /* Taille des étoiles */
  cursor: pointer;
}
.star {
  display: inline-block;
}
.star svg {
  width: 24px;
  height: 24px;
}
</style>
