<template>
  <div class="accordion">
    <div v-for="(item, index) in items" :key="index" class="accordion-item">
      <div
        class="accordion-title flex items-center justify-between"
        :class="{ active: isOpen(index) }"
        @click="toggle(index)"
      >
        <span>{{ item.title }}</span>
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="w-6 h-6 transition-transform duration-300 transform" :class="{ 'rotate-180': isOpen(index) }">
          <path fill="currentColor" d="M7 10l5 5 5-5z" />
        </svg>
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
  border-radius: 4px 4px 0 0;
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

.rotate-180 {
  transform: rotate(180deg);
}
</style>
