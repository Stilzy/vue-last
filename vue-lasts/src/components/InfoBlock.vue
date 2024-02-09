<script setup>
    defineProps({
        title: String,
        description: String,
        imageUrl: String,
    })
</script>


<template>
    <div class="glav__info">
        <img class="info__img" :src="imageUrl" alt="">
        <h2 class="info__titles">{{ title }}</h2>
        <p class="desc__info">{{ description }}</p>
    </div>
</template>