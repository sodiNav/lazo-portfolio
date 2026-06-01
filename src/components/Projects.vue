<script setup>
    import projects from '../data/projects.json';
    import ProjectCard from './ProjectCard.vue';
    import { reactive } from 'vue';

    const projectData = reactive(projects);
</script>

<template>
    <!-- Projects Section -->
     
    <section>
        <div id="projects" class="row bg-light-custom py-md-5 px-md-5 justify-content-center">
            <h2 class="text-center header-font mb-5">My Projects</h2>
            <ProjectCard v-for="project in projectData" :project="project" />
        </div>
    </section>

    <!-- End of Projects Section -->
</template>

<style scoped></style>
