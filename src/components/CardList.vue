<script setup>
import Card from './Card.vue'

const onClickAdd = () =>{
  alert("Добавить");
}

</script>

<template>
    <div class="grid grid-cols-4 gap-5">
        <Card title="Мужские кроссовки Nike Blazer Mid Suede" :price="1205" imgUrl="/sneakers/sneakers-1.jpg" :is-favorite="false" :onClickAdd="onClickAdd"/>
      </div>
</template>