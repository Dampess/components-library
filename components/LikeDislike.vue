<template>
  <div class="reaction-system">
    <div v-for="(icon, index) in icons" :key="index" @click="toggleReaction(index)" :class="{ active: isActive(index) }">
      <img :src="icon.url" :alt="icon.label" :title="icon.label">
    </div>
  </div>
</template>

<script>
export default {
  props: {
    customIcons: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      reactions: []
    };
  },
  computed: {
    icons() {
      return this.customIcons.length > 0 ? this.customIcons : defaultIcons;
    }
  },
  methods: {
    toggleReaction(index) {
      if (this.reactions.includes(index)) {
        this.reactions = this.reactions.filter(r => r !== index); // Remove reaction
      } else {
        this.reactions.push(index); // Add reaction
      }
    },
    isActive(index) {
      return this.reactions.includes(index);
    }
  }
};

const defaultIcons = [
  { url: 'like.png', label: 'Like' },
  { url: 'dislike.png', label: 'Dislike' },
  { url: 'love.png', label: 'Love' }
];
</script>

<style scoped>
.reaction-system {
  display: flex;
}

.reaction-system div {
  cursor: pointer;
  margin-right: 10px;
}

.reaction-system div.active {
  opacity: 0.5; /* Example: reduce opacity for active reactions */
}
</style>
