<script setup>
    import DrawerHeader from './DrawerHeader.vue'
    import CartItemList from './CartItemList.vue'
    import InfoBlock from './InfoBlock.vue'

    const emit = defineEmits(['createOrder'])


    defineProps({
        totalPrice: Number,
        isCreateingOrder: Boolean,
    })

</script>



<template>
    <div class="drawer__rod"></div>
    <div class="drawers">
        <DrawerHeader/>


        <InfoBlock v-if="!totalPrice" title="Корзина пустая" description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ." image-url="./package-icon.png"/>

        <CartItemList v-if="totalPrice"/>


        <div v-if="totalPrice">
            <div class="down__draweers">
                <span>Итого:</span>
                <div class="gran"></div>
                <b>{{ totalPrice }} Руб.</b>
            </div>
            <button @click="() => emit('createOrder')" class="button__ofofrm">
                Оформить заказ
            </button>
        </div>
    </div>
</template>