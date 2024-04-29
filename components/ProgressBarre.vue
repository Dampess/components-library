<template>
  <div class="relative w-56 mx-auto">
    <div class="relative w-full h-6 bg-gray-200 rounded-lg overflow-hidden">
      <div class="absolute top-0 left-0 h-full bg-blue-500 transition-all duration-300" :style="{ width: progress + '%' }"></div>
    </div>
    <div class="absolute top-0 left-0 w-full h-full flex items-center justify-center text-sm text-gray-700 font-semibold">{{ progress }}%</div>
  </div>
</template>

<script>
export default {
  props: {
    progress: {
      type: Number,
      required: true,
      validator: value => value >= 0 && value <= 100
    },
    color: {
      type: String,
      default: 'blue-500'
    },
    height: {
      type: String,
      default: 'h-6'
    }
  }
};
</script>
