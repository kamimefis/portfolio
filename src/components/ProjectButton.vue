<script lang="ts">
import { defineComponent } from "vue";
import type { PropType } from 'vue'

export default defineComponent({
    props: {
        onClick: Function as PropType<(event: MouseEvent) => void>,
        iconPath: String,
        ariaLabel: String,
    },
});
</script>

<template>
    <button @click="onClick" type="button" class="text-white bg-blue-800 hover:bg-blue-400 focus:ring-4 focus:outline-none focus:ring-blue-300 
           font-medium rounded-lg text-sm p-2.5 text-center inline-flex items-center me-2 
           dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
        <svg class="w-6 h-6 text-gray-800 dark:text-white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
            width="24" height="24" fill="none" viewBox="0 0 24 24">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" :d="iconPath" />
        </svg>
        <span class="sr-only">{{ ariaLabel }}</span>
    </button>
</template>