<template>
    <div class="item" @click="handleClick">
        <input type="radio" name="lvl" :id="id" :value="label">
        <label :for="id">{{ label }}</label>
    </div>
</template>

<script setup>
    import { defineProps, defineEmits  } from 'vue';

    const emit = defineEmits(['click']);

    const props = defineProps({
        label: String,
        gameSpeed: Number,
        id:String
    })

    const handleClick = () => {
        emit('click', { gameSpeed: props.gameSpeed });
    };
</script>
