<script setup>
    import { inject } from 'vue'
import CartItem from './CartItem.vue'

    const { cart, removeFromCart } =inject('cart')
</script>



<template>
    <div class="card__blockse">
        <CartItem v-for="item in cart" 
        :key="item.id" 
        :title="item.title" 
        :price="item.price"
        :imageUrl="item.imageUrl"
        @on-click-remove="() => removeFromCart(item)"
        />
    </div>
</template>