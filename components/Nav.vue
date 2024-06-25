<template>
  <nav>
      <ul>
        <li :class="{ 'active': isActive('/') }" class="text-2xl">
          <NuxtLink to="/">
            <i class="fas fa-home text-white"></i>
          </NuxtLink>
        </li>
        <li :class="{ 'active': isActive('/articlePreview') }" class="text-2xl">
          <NuxtLink to="/articlePreview">
            <i class="fas fa-book text-white"></i>
          </NuxtLink>
        </li>
        <li v-if="isLoggedIn" :class="{ 'active': isActive('/manageArticle') }" class="text-lg p-1">
          <NuxtLink to="/manageArticle">My Articles</NuxtLink>
        </li>
        <li>
          <button @click="toggleAuthPopup" class="focus:outline-none p-4 hover:bg-gray-900 text-2xl">
            <i class="fas fa-user text-white"></i>
          </button>
        </li>
      </ul>
      <div v-if="showAuthPopup" class="absolute right-0 w-48 bg-white-cstm shadow-lg rounded-md z-50 mr-0 mt-0 font-bold p-4 cursor-pointer hover:bg-gray-200">
        <div v-if="isLoggedIn" @click="logout">Logout</div>
        <div v-else @click="redirectToLogin">Login</div>
      </div>
  </nav>
</template>

<script>
import { useRoute } from 'vue-router';

export default {
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
    // console.log("token:", localStorage.getItem('token'));
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
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

nav {
  position: fixed; 
  top: 0;
  width: 100%; 
  background-color: #333; 
  z-index: 1000;
  font-family: 'Roboto', sans-serif;
}

ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
    overflow: hidden;
    background-color: #333;
    height: 60px;
    align-content: center;
  }
  
  li {
    float: left;
  }
  
  li a {
    display: block;
    color: white;
    text-align: center;
    padding: 14px 16px;
    text-decoration: none;
  }
  
  li a:hover {
    background-color: #111;
  }
  
  li:last-child {
    float: right;
  }
  
  .active {
    background-color: #7e807f;
  }

  .bg-white-cstm {
    background-color: #f9f9f9;
  }
</style>
