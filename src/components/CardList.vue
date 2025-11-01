<script setup>
import Card from "./Card.vue";

defineProps({
  items: Array,
  isFavorites: Boolean,
});

const emit = defineEmits(["addToFavorite", "addToCart"]);
</script>

<template>
  <div v-auto-animate class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :price="item.price"
      :imgUrl="item.imageUrl"
      :onClickAdd="isFavorites ? null : () => emit('addToCart', item)"
      :onClickFav="isFavorites ? null : () => emit('addToFavorite', item)"
      :is-favorite="item.isFavorite"
      :isAdded="item.isAdded"
    />
  </div>
</template>
