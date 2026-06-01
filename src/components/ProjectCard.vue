<script setup>
    import { defineProps } from 'vue';

    defineProps({
        project: Object
    })
</script>

<template>
    <div class="col-sm-12 col-md-6 col-lg-4 mb-5">
        <div class="card project h-100">
            <div class="card-body">
                <img :src="project.image" alt="" class="img-fluid mb-4">
                <h3 class="card-title">{{ project.title }}</h3>
                <p class="card-text">{{ project.description }}</p>
            </div>
        </div>
    </div>
</template>

<style scoped></style>
