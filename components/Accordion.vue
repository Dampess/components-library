<template>
  <div class="accordion">
    <div v-for="(item, index) in items" :key="index" class="accordion-item">
      <div
        class="accordion-title"
        :class="{ active: isOpen(index) }"
        @click="toggle(index)"
      >
        {{ item.title }}
      </div>
      <div v-if="isOpen(index)" class="accordion-content">
        {{ item.content }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      required: true,
    },
    exclusive: {
      type: Boolean,
      default: false, // Pour ouvrir une seule section à la fois
    },
  },
  data() {
    return {
      openIndexes: [], // Indice(s) des sections ouvertes
    };
  },
  methods: {
    toggle(index) {
      if (this.exclusive) {
        // Si le mode exclusif est activé, fermez toutes les autres sections
        if (this.isOpen(index)) {
          this.openIndexes = [];
        } else {
          this.openIndexes = [index];
        }
      } else {
        // Si le mode exclusif est désactivé, basculez simplement l'état de la section
        const isOpen = this.isOpen(index);
        if (isOpen) {
          this.openIndexes = this.openIndexes.filter((i) => i !== index);
        } else {
          this.openIndexes.push(index);
        }
      }
    },
    isOpen(index) {
      return this.openIndexes.includes(index);
    },
  },
};
</script>

<style scoped>
.accordion {
  width: 100%;
}

.accordion-item {
  margin-bottom: 1rem;
}

.accordion-title {
  cursor: pointer;
  padding: 1rem;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.accordion-title.active {
  background-color: #e0e0e0;
}

.accordion-content {
  padding: 1rem;
  background-color: #fafafa;
  border: 1px solid #ccc;
  border-top: none;
  border-radius: 0 0 4px 4px;
}
</style>
