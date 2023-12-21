<script setup>
import { defineProps, computed } from 'vue';

const props = defineProps({
    alert: Object,
    message: String
});

const isError = computed(() => props.alert.type === 'error');
</script>

<template>
    <!-- :class para que sea dinamico -->
    <div :class="[isError ? 'bg-red-500' : 'bg-green-500']"
        class="text-white text-center py-3 mb-3 px-4 rounded-md font-bold uppercase">
        {{ alert.message }}
    </div>
</template>