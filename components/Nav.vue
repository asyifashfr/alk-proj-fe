<template>
  <nav>
      <ul>
        <li :class="{ 'active': isActive('/') }"><a href="/">Home</a></li>
        <li v-if="isLoggedIn" :class="{ 'active': isActive('/manageArticle') }">
          <a href="/manageArticle">Manage Articles</a>
        </li>
        <li v-if="isLoggedIn"><a href="#" @click="logout">Logout</a></li>
        <li v-else><a href="/login">Login</a></li>
      </ul>
  </nav>
</template>

<script>
import { useRoute } from 'vue-router';

export default {
data() {
  return {
    isLoggedIn: true
  };
},
setup() {
  const route = useRoute();
  
  function isActive(path) {
    return route.path === path;
  }
  
  return { isActive };
},
mounted() {
  this.checkLogin();
},
methods: {
  checkLogin() {
    console.log("token:", localStorage.getItem('token'));
    this.isLoggedIn = !!localStorage.getItem('token');
  },
  logout() {
    localStorage.removeItem('token');
    this.isLoggedIn = false;
    this.$router.go(); 
  }
}
}
</script>

<style scoped>
@import '~/assets/css/nav.css';
</style>
