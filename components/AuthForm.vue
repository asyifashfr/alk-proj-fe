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
      return `http://localhost:8080/user/${this.isLogin ? 'login' : 'register'}`;
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

<style scoped>
  *,
  *:before,
  *:after{
      padding: 0;
      margin: 0;
      box-sizing: border-box;
  }

  form{
      width: 420px;
      display: flex;
      flex-direction: column;
      background-color: #5d5d5d;
      position: absolute;
      transform: translate(-50%,-50%);
      top: 50%;
      left: 50%;
      border-radius: 10px;
      backdrop-filter: blur(10px);
      border: 2px solid rgba(255,255,255,0.1);
      padding: 40px 40px 40px 40px;
  }

  form *{
      font-family: 'Poppins',sans-serif;
      color: #ffffff;
      letter-spacing: 0.5px;
      outline: none;
      border: none;
  }

  form h3{
      font-size: 32px;
      font-weight: 800;
      line-height: 42px;
      text-align: center;
  }

  label{
      display: block;
      margin-top: 25px;
      font-size: 16px;
      font-weight: 500;
  }

  input{
      display: block;
      height: 50px;
      width: 100%;
      background-color: rgba(0, 0, 0, 0.07);
      border-radius: 3px;
      padding: 0 10px;
      margin-top: 8px;
      font-size: 14px;
      font-weight: 300;
  }

  ::placeholder{
      color: #c2c2c2;
  }

  button{
      margin-top: 40px;
      width: 100%;
      background-color: #ffffff;
      color: #080710;
      padding: 15px 0;
      font-size: 18px;
      font-weight: 600;
      border-radius: 5px;
      cursor: pointer;
  }

  button:hover{
      background-color: #eaeaea;
  }

  .hyperlink{
      display: block;
      text-align: center;
      margin-top: 25px;
      font-size: 14px;
      font-weight: 500;
      color: #ffffff;
      text-decoration: none;
  }
</style>
  