<script setup>
const emit = defineEmits(['onClickRemove'])


    defineProps({
        id: Number,
        title: String,
        imageUrl: String,
        price: Number,
    })
</script>


<template>
    <div class="cart__items">
        <img class="item__img" :src="imageUrl" alt="">
        <div class="items__info">
            <p>{{ title }}</p>
            
            <div class="block__items__down">
                <b>{{ price }}</b>
                <img @click="emit('onClickRemove')" src="/close.svg" alt="">
            </div>
        </div>
    </div>
</template>