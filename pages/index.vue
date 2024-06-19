<template>
    <div class="container mx-auto px-4 py-8">
      <header class="text-lg font-bold mb-8 text-center">
        Our Articles
      </header>
      <div class="flex flex-col lg:flex-row gap-8">
        <!-- Article Preview -->
        <div class="w-full lg:w-3/5" v-if="articles && articles.length > 0">
          <div class="p-6 bg-white rounded-lg shadow-md">
            <h2 class="text-lg font-semibold mb-1 text-center">{{ mainArticle.title }}</h2>
            <span class="text-gray-600 text-sm block mb-4 text-center">by {{ mainArticle.username }}</span>
            <div class="text-gray-700 mb-4" v-html="mainArticle.content">
            </div>
          </div>
        </div>
        <!-- All rticles -->
        <div class="w-full lg:w-2/5 space-y-4 cursor-pointer" v-if="articles && articles.length > 0">
          <div v-for="(article, index) in articles" :key="index" class="p-4 bg-white rounded-lg shadow-md"  @click="setMainArticle(article)">
            <h3 class="text-md font-medium mb-1">{{ article.title }} <span class="text-gray-400 text-sm ml-1">by {{ articles[0].username }}</span></h3>
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
        articles: [],
        mainArticle: null
      };
    },
    async mounted() {
      try {
        const response = await fetch('http://localhost:8080/article/all');
        this.articles = await response.json();
        this.mainArticle = this.articles[0];
        console.log(this.articles)
      } catch (error) {
        console.error(error);
      }
    },
    methods: {
      truncateText(text, lim = 200) {
        return text.length > lim ? text.slice(0, lim) + '...' : text;
      },
      setMainArticle(article) {
        console.log("as")
        this.mainArticle = article;
      }
    }
  }
  </script>
  
  <style lang="scss" scoped>
  </style>
  