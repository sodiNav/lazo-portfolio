<script setup>

  import Navbar from './components/Navbar.vue';
  import BackToTopComponent from './components/BackToTopComponent.vue';
  import Landing from './components/Landing.vue';
  import About from './components/About.vue';
  import Projects from './components/Projects.vue';
  import Tools from './components/Tools.vue';
  import Contacts from './components/Contacts.vue';
  import FooterComponent from './components/FooterComponent.vue';

</script>

<template>
  <Navbar />

  <BackToTopComponent />

  <main class="container-fluid">
    <Landing />
    <About />
    <Projects />
    <Tools />
    <Contacts />
  </main>

  <FooterComponent />
</template>

<style scoped></style>
