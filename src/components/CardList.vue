<script setup>
import Card from './Card.vue'

defineProps({
  items:Array
});

const emit = defineEmits(['addToFavorite','addToCart']);


</script>

<template>
    <div class="grid grid-cols-4 gap-5">
        <Card 
        v-for="item in items"
          :key="item.id"
          :id="item.id"
          :title="item.title" 
          :price="item.price" 
          :imgUrl="item.imageUrl"
          :onClickAdd="() => emit('addToCart',item)"
          :onClickFav="() => emit('addToFavorite',item)"
          :is-favorite="item.isFavorite"
          :isAdded="item.isAdded"
        />
      </div>
</template>