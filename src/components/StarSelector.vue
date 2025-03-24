<template>
  <div class="star-selector">
    <div
      class="star-icon"
      v-for="n in 5"
      :key="n"
      @click="updateStars(n)"
    >
      <font-awesome-icon v-if="modelValue >= n" class="Star" :icon="['fas', 'star']" size="lg" style="color: #FFD43B;" />
      <font-awesome-icon v-else class="Star" :icon="['fad', 'star']" size="lg" />
    </div>
  </div>
</template>

<script setup>
defineProps({
  modelValue: {
    type: Number,
    required: true
  }
})

const emit = defineEmits(['update:modelValue'])

function updateStars(n) {
  emit('update:modelValue', n)
}
</script>

<style scoped lang="scss">
.star-selector {
    display: flex;
    align-content: center;
    justify-content: space-between;

    .star-icon{
      cursor: pointer;
    }

    .StarPicker{
      display: inline-flex;
      align-items: center;

      .StarTitle{
        margin-right: 12px;
      }
    }
}
</style>