<script setup>
import { inject } from 'vue';
import CartItem from './CartItem.vue';

const {cart}=inject(['cart']);

</script>


<template>
    <div class="flex flex-col flex-1 gap-4 justify-between">
        <CartItem v-for="item in cart" :key="item.id" :title="item.title" :price="item.price" :img-url="item.imgUrl"/>
      </div>
</template>