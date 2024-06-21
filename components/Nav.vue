<template>
  <nav>
      <ul>
        <li :class="{ 'active': isActive('/') }">
          <a href="/">
            <i class="fas fa-home text-white"></i>
          </a>
        </li>
        <li v-if="isLoggedIn" :class="{ 'active': isActive('/manageArticle') }">
          <a href="/manageArticle">Manage Articles</a>
        </li>
        <li class="ml-4">
          <a href="/searchPage">
            <i class="fas fa-search text-white"></i>
          </a>
        </li>
        <li>
          <button @click="toggleAuthPopup" class="focus:outline-none p-4">
            <i class="fas fa-user text-white"></i>
          </button>
        </li>
      </ul>
      <div v-if="showAuthPopup" class="absolute right-0 mt-2 w-48 bg-white shadow-lg rounded-lg z-50">
        <div v-if="isLoggedIn" @click="logout" class="p-4 cursor-pointer hover:bg-gray-200 rounded-lg">Logout</div>
        <div v-else @click="redirectToLogin" class="p-4 cursor-pointer hover:bg-gray-200 rounded-lg">Login</div>
      </div>
  </nav>
</template>

<script>
import { useRoute } from 'vue-router';

export default {
props: {
  searchQuery: {
    type: String,
    required: true
  },
  filterArticles: {
    type: Function,
    required: true
  }
  },
data() {
  return {
    isLoggedIn: true,
    showAuthPopup: false
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
  },
  redirectToLogin() {
    this.$router.push('/login');
  },
  toggleAuthPopup() {
    this.showAuthPopup = !this.showAuthPopup;
  }
}
}
</script>

<style scoped>
@import '~/assets/css/nav.css';
</style>
