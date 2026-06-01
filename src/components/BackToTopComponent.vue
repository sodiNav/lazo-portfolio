<script setup>
    import { ref, onMounted, onUnmounted } from 'vue';

    const isActive = ref(false);

    const handleScroll = () => {
        isActive.value = window.pageYOffset > 100;
    }

    onMounted(() => {
        window.addEventListener('scroll', handleScroll);
    })
</script>

<template>
    <a href="#landing" class="to-top text-decoration-none" :class="{ active: isActive}">
        <i class="bi bi-arrow-up-circle-fill"></i>
    </a>
</template>

<style scoped></style>
