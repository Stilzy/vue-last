<script setup>
    import Card from './Card.vue'

    defineProps({
        items: Array
    })

    const emit = defineEmits(['addToCart'])

</script>



<template>
    <div class="card__blockss">
        <Card 
        v-for="item in items"
        :key="item.id"
        :id="item.id"
        :title="item.title" 
        :imageUrl="item.imageUrl" 
        :price="item.price" 
        :isFavorite="item.isFavorite"
        :onClickAdd="() => emit('addToCart', item)"
        :isAdded="item.isAdded"
        />
    </div>
</template>