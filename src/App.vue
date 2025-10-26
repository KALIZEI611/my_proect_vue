<script setup>
import { onMounted,provide,reactive,ref, watch } from "vue";
import axios from "axios";
import Header from "./components/Header.vue";
import CardList from "./components/CardList.vue";
import Drawer from "./components/Drawer.vue";

const items = ref([]);

const filters = reactive({
  sortBy:'title',
  searchQuery:''
});

const onChangeSelect = (e)=>{
  filters.sortBy = e.target.value;
}

const onChangeSearchInput=(e)=>{
  filters.searchQuery=e.target.value;
}

const addToFavorite = async (item) =>{
  try{
   if(!item.isFavorite){
     const obj = {
      parentId:item.id
    };
    item.isFavorite=true;
    const {data} = await axios.post("https://5c4f68a7b58c636d.mokky.dev/favorites",obj);
    item.favoritesId=data.id;
   } else{
      item.isFavorite=false;
      await axios.delete(`https://5c4f68a7b58c636d.mokky.dev/favorites/${item.favoritesId}`);
      item.favoritesId=null;
    }
  }
  catch(e){
    console.log(e);
  }
}

const fetchFavorites=async()=>{
  try{
    const {data:favorites} = await axios.get("https://5c4f68a7b58c636d.mokky.dev/favorites"); 
    items.value=items.value.map((item)=>{
      const favorite = favorites.find((favorite)=>favorite.parentId===item.id);
      if(!favorite){
        return item
      }
      return{
        ...item,
        isFavorite:true,
        favoritesId:favorite.id
      }
    });
  }
  catch (e){
    console.log(e);
  }
}

const fetchItems = async () => {
   try{
    const params = {
      sortBy:filters.sortBy
    };
    if(filters.searchQuery){
      params.title = `*${filters.searchQuery}*`;
    }
    const {data} = await axios.get("https://5c4f68a7b58c636d.mokky.dev/items",{params});
    items.value=data.map((obj) => ({
      ...obj,
      isFavorite:false,
      favoritesId:null,
      isAdded:false,
    }));
  }
  catch (e){
    console.log(e);
  }
}

onMounted(async()=>{
  await fetchItems();
  await fetchFavorites();
});
watch(filters,fetchItems);


provide('addToFavorite',addToFavorite);
</script>

<template>
  <!--<Drawer /> -->
  <div class="bg-white w-4/5 m-auto  rounded-xl shadow-xl mt-14">
    <Header />
    <div class="p-10">
      <div class="flex justify-between items-center mb-10">
        <h1 class="text-3xl font-bold">Все кроссовки</h1>
        <div class="flex items-center gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 border border-gray-200 focus:border-gray-400 rounded-md focus:outline-none">
            <option value="name">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>
          </select>
          <div class="relative">
            <input
              @input="onChangeSearchInput"
              type="text"
              class="border border-gray-200 rounded-md py-2 pl-10 pr-4 focus:outline-none focus:border-gray-400"
              placeholder="Поиск..."
            />
            <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
              <img src="/search.svg" />
            </div>
          </div>
        </div>
      </div>
      <div class="mt-10"> 
        <CardList :items="items" @addToFavorite="addToFavorite"/>
      </div>
    </div>
    
    

  </div>
</template>

<style scoped></style>
