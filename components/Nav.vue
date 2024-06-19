<template>
    <nav>
        <ul>
          <li><a href="/" class="active">Home</a></li>
          <li v-if="isLoggedIn"><a href="/">Manage Articles</a></li>
          <li v-if="isLoggedIn"><a href="#" @click="logout">Logout</a></li>
          <li v-else><a href="/login">Login</a></li>
        </ul>
    </nav>
</template>

<script>
export default {
  data() {
    return {
      isLoggedIn: false
    };
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



<style>
  @import '~/assets/css/nav.css';
</style>
  