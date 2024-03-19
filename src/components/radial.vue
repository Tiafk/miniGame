<template>
    <div class="circle">
        <span v-for="(color, index) in colors" :key="index" :class="['item', color, { active: index === currentIndex }]" @click="colorClick(index + 1)"></span>
    </div>
</template>

<script setup>
    import { defineProps, defineEmits } from 'vue';

    const emit = defineEmits(['colorClick'])

    const props = defineProps({
        colors: {
            type: Array
        },
        currentIndex: {
            type: Number
        }
    })

    const colorClick = (index) => {
        emit('colorClick', index)
    }
</script>

<style lang="scss">

</style>