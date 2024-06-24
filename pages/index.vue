<template>
    <div class="mx-auto" style="min-height: 100vh">
      <!-- Welcome Section -->
      <div class="bg-gradient-to-r from-yellow-300 to-yellow-600 text-white p-4 mb-4 w-full flex flex-col justify-end" style="height: 300px;">
        <h1 class="text-5xl font-bold p-4">WELCOME BACK!</h1>
        <p class="text-2xl mb-8 p-4">We're thrilled to have you here! Dive into these engaging articles and enjoy your reading journey.</p>
      </div>
      <!-- Check This Out Section -->
      <div class="text-center mb-8">
        <h2 class="text-3xl font-bold flex justify-start p-8 ml-4">Check This Out</h2>
        <div class="flex justify-around mt-4 gap-8 mr-10 ml-10 mb-8">
          <div v-for="(article, index) in randomArticles" :key="index" class="w-1/3 p-4 bg-white shadow-lg rounded-lg hover:shadow-2xl border border-black" @click="goToArticlePreview(article)">
            <h3 class="text-lg font-semibold mb-1">{{ article.title }}</h3>
            <span class="text-gray-600 text-sm block mb-4">by {{ article.username }}</span>
            <p class="text-gray-700">{{ truncateText(article.content, 100) }}</p>
          </div>
        </div>
        <button @click="goToArticlePreview" class="text-black underline py-2 px-4 rounded">
          See More
        </button>
      </div>
      <!-- Write Your Own Section -->
      <div class="bg-black text-white text-center p-8 pb-20">
        <h2 class="text-3xl font-bold mb-4">Write Your Own Article</h2>
        <p class="text-lg mb-8">Share your thoughts and stories with our community. Create your own article now!</p>
        <button @click="goToLogin" class="bg-gradient-to-r from-yellow-400 to-yellow-600 text-white text-bold py-2 px-4 rounded">
          Start Writing
        </button>
      </div>
    </div>
  </template>
  
<script>
  export default {
    data() {
      return {
        articles: [],
        randomArticles: [],
        isAuthenticated: false
      };
    },
    async mounted() {
      const token = localStorage.getItem('token');
      if (token) {
        this.isAuthenticated = true;
      }

      try {
        const response = await fetch('http://localhost:8080/article/all');
        this.articles = await response.json();
        console.log(this.articles);
        this.randomArticles = this.getRandomArticles(3);
      } catch (error) {
        console.error(error);
      }
    },
    methods: {
      truncateText(text, lim = 200) {
        return text.length > lim ? text.slice(0, lim) + '...' : text;
      },
      getRandomArticles(count) {
        let shuffled = [...this.articles].sort(() => 0.5 - Math.random());
        return shuffled.slice(0, count);
      },
      goToArticlePreview() {
        this.$router.push('/articlePreview');
      },
      goToLogin() {
        this.$router.push('/login');
      }
    }
  }
  </script>
  
  <style scoped>
      @import '~/assets/css/home.css';
  </style>
  