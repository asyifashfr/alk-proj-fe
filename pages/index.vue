<template>
    <div class="container mx-auto px-4 py-8">
      <header class="text-lg font-bold mb-8 text-center">
        Our Articles
      </header>
      <div class="flex flex-col lg:flex-row gap-8">
        <!-- Latest Article Preview -->
        <div class="w-full lg:w-3/5" v-if="articles && articles.length > 0">
          <div class="p-6 bg-white rounded-lg shadow-md">
            <h2 class="text-lg font-semibold mb-1 text-center">{{ articles[0].title }}</h2>
            <span class="text-gray-600 text-sm block mb-4 text-center">by altha</span>
            <p class="text-gray-700 mb-4">
              {{ truncateText(articles[0].content) }}
            </p>
            <button class="text-blue-500 hover:underline">Read More</button>
          </div>
        </div>
        <!-- Other Articles -->
        <div class="w-full lg:w-2/5 space-y-4" v-if="articles && articles.length > 1">
          <div v-for="(article, index) in articles.slice(1)" :key="index" class="p-4 bg-white rounded-lg shadow-md">
            <h3 class="text-md font-medium mb-1">{{ article.title }} <span class="text-gray-400 text-sm ml-1">by john_doe</span></h3>
            <p class="text-gray-600 text-sm">{{ truncateText(article.content) }}</p>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        articles: []
      };
    },
    async mounted() {
      try {
        console.log("yeay")
        const response = await fetch('http://localhost:8080/articles');
        this.articles = await response.json();
        console.log(this.articles)
      } catch (error) {
        console.error(error);
      }
    },
    methods: {
      truncateText(text) {
        return text.length > 200 ? text.slice(0, 200) + '...' : text;
      }
    }
  }
  </script>
  
  <style lang="scss" scoped>
<<<<<<< HEAD
=======

>>>>>>> a013fdb8e8aeed52709da35480d10ca09f48701e
  </style>
  