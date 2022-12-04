<script setup>
const props = defineProps({
  imageUrl: { type: String, required: true },
  loading: { type: Boolean, required: true },
})

const getImageUrl = (name) => {
  return new URL(`../assets/${name}`, import.meta.url).href
}

</script>

<template>
  <div>
        <img :src="getImageUrl('spinner.svg')" class="symbol-item__image" alt="spinner" v-if="loading">
        <img :src="getImageUrl(props.imageUrl)" class="symbol-item__image" alt="symbol" v-else/>
  </div>
</template>

<style scoped>
.symbol-item__image{
  max-width: 100%;
  max-height: 100%;
}
</style>
