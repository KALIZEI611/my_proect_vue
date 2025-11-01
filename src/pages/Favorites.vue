<script setup>
import { onMounted, ref } from "vue";
import axios from "axios";
import CardList from "../components/CardList.vue";

const favorites = ref([]);

onMounted(async () => {
  try {
    const { data } = await axios.get(
      "https://5c4f68a7b58c636d.mokky.dev/favorites?_relations=items"
    );

    favorites.value = data.map((obj) => obj.item);
  } catch (e) {
    console.log(e);
  }
});
</script>
<template>
  <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>
  <CardList :items="favorites" is-Favorites="" />
</template>
