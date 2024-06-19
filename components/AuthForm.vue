<template>
    <div>
      <form @submit.prevent="handleSubmit">
        <h3>{{ isLogin ? 'Login' : 'Register' }}</h3>
        <label for="username">Username</label>
        <input v-model="user.username" type="text" name="username" placeholder="Username">
  
        <label v-if="!isLogin" for="email">Email</label>
        <input v-if="!isLogin" v-model="user.email" type="email" name="email" placeholder="Email">
  
        <label for="password">Password</label>
        <input v-model="user.password" type="password" name="password" placeholder="Password">
  
        <button type="submit">{{ isLogin ? 'Login' : 'Register' }}</button>
        <div class="hyperlink">
          <a v-if="isLogin" href="/register">New user? Register</a>
          <a v-else href="/login">Already have an account? Log In</a>
        </div>
      </form>
    </div>
</template>
  
<script>
export default {
  props: {
    mode: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      user: {
        username: '',
        email: '',
        password: ''
      }
    };
  },
  computed: {
    isLogin() {
      return this.mode === 'login';
    },
    apiUrl() {
      return `http://localhost:8080/users/${this.isLogin ? 'login' : 'register'}`;
    }
  },
  methods: {
    async handleSubmit() {
      if (!this.user.username || (!this.isLogin && !this.user.email) || !this.user.password) {
        alert('Please fill in all fields');
        return;
      }

      try {
        const response = await fetch(this.apiUrl, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(this.user)
        });

        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        const responseData = await response.json();
        console.log(responseData);

        if (this.isLogin && responseData.token) {
          localStorage.setItem('token', responseData.token);
          this.$router.push('/');
        } else {
          this.$router.push('/login');
        }
      } catch (error) {
        console.error('Error:', error.message);
        alert(error.message);
      }
    }
  }
}
</script>

<style>
  @import '~/assets/css/auth.css';
</style>
  