<script setup>
  defineProps({
    totalPrice: Number,
    loggedInUser: Function,
    name: Function,
    login: Function,
    addLogins: Function,
  })

  const emit = defineEmits(['openDrawer']);
</script>




<template>
    <header class="header">

<div class="left__block">
  <img class="logo__header" src="/logo.png" alt="">
  <div>
    <h2 class="">Sneakers</h2>
    <p>Магазин лучших кроссовок</p>
  </div>
  <p>{{addLogins(loggedInUser)}}</p>
  <ul class="right__block">
  <li @click="() => emit('openDrawer')" class="right__blocks">
    <img src="/cart.svg" alt="">
    <b>{{ totalPrice }}</b>
  </li>
</ul>
</div>

</header>
</template>