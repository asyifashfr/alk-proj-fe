<template>
    <div>
      <Nav v-if="showNavbar" />
      <NuxtPage />
    </div>
  </template>
  
  <script>
  import Nav from '~/components/Nav.vue';
  
  export default {
    components: {
      Nav
    },
    computed: {
      showNavbar() {
        const hideNavbarRoutes = ['/login', '/register'];
        return !hideNavbarRoutes.includes(this.$route.path);
      }
    }
  };
  </script>
  
  <style scoped>
  </style>
  