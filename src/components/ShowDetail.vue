<!-- Modal.vue -->
<template>
<div v-if="isOpen" class="fixed inset-0 flex items-center justify-center">
    <div id="default-modal" tabindex="-1" aria-hidden="true" class="overflow-y-auto overflow-x-hidden fixed top-0 right-0 left-0 z-50 justify-center items-center w-full md:inset-0 h-[calc(100%-1rem)] max-h-full flex">
        <div class="relative p-4 w-full max-w-2xl max-h-full">
            <!-- Your modal content goes here -->
            <slot></slot>
        </div>
    </div>
</div>
</template>

<script>
export default {
    data() {
        return {
            isOpen: false,
        };
    },
    methods: {
        openModal() {
            this.isOpen = true;
        },
        closeModal() {
            this.isOpen = false;
        },
    },
};
</script>

<style scoped>
/* Add your custom styles here */
</style>
