<script setup>
  defineProps({
    title: String,
    imageUrl: String,
    price: Number,
    isFavorite: Boolean,
    isAdded: Boolean,
    onClickAdd: Function,
    onClickFavorite: Function,
  })
</script>



<template>
    <div>
      <div class="card">
        <img :src="imageUrl" alt="">
        <p>{{ title }}</p>

        <div class="down__line__card">
          <div class="price__block">
            <span>Цена:</span>
            <b>{{ price }}руб.</b>
          </div>

          <img @click="onClickAdd" :src="!isAdded ? './plus.svg' : './checked.svg'" alt="">
        </div>
      </div>
    </div>
</template>