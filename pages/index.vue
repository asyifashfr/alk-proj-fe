<template>
    <div class="container mx-auto px-4 py-8">
      <div class="flex flex-col lg:flex-row gap-8">
        <!-- Article Preview -->
        <div class="w-full lg:w-3/5" v-if="articles && articles.length > 0">
          <div class="p-6 bg-white">
            <h2 class="text-lg font-semibold mb-1 text-center">{{ mainArticle.title }}</h2>
            <span class="text-gray-600 text-sm block mb-4 text-center">by {{ mainArticle.username }}</span>
            <div class="text-gray-700 mb-4" v-html="mainArticle.content"></div>
            <hr class="border-t border-gray-200 my-4">
            <div class="flex space-x-4 text-gray-600">
              <div class="flex items-center space-x-2">
                <button @click="likeArticle(mainArticle)" class="p-2 rounded-md hover:bg-gray-200">
                  <i class="fas fa-heart"></i>
                </button>
                <span>{{ mainArticle.likes }}</span>
              </div>
              <div class="flex items-center space-x-2">
                <button class="p-2 rounded-md hover:bg-gray-200">
                  <i class="fas fa-comment"></i>
                </button>
                <span>{{ comments.length }}</span>
              </div>
            </div>
            <div>
              <h3 class="text-lg mb-2 mt-4"><strong>Comments</strong></h3>
              <div v-for="(comment, index) in comments" :key="index" class="mb-2">
                <p class="text-gray-800 font-semibold">{{ comment.user_id }}</p>
                <p class="text-gray-600">{{ comment.content }}</p>
              </div>
              <div v-if="isAuthenticated" class="mt-4 flex items-center space-x-2">
                <input
                  v-model="newComment"
                  type="text"
                  placeholder="Add a comment..."
                  class="w-full p-2 border border-gray-300 rounded-full mt-1 p-4"
                />
                <button @click="addComment(mainArticle)" class="bg-black text-white py-2 px-4 rounded-full hover:bg-gray-600 mt-2">
                  <i class="fas fa-paper-plane"></i>
                </button>
              </div>
              <div v-else class="mt-4 text-gray-600">
                Please log in to add a comment.
              </div>
            </div>          
          </div>
        </div>
        <!-- All rticles -->
        <div class="lg:w-2/5 space-y-4 cursor-pointer max-h-maxh-screen">
          <header class="text-2xl text-white font-bold text-center  rounded-md sticky top-0 bg-black z-10 p-4">
            Our Articles
          </header>
          <div class="w-full space-y-4 cursor-pointer overflow-y-auto max-h-maxh-screen" v-if="articles && articles.length > 0">
            <div v-for="(article, index) in articles" :key="index" class="p-4 bg-white rounded-lg shadow-md"  @click="setMainArticle(article)">
              <h3 class="text-md font-medium mb-1">{{ article.title }} <span class="text-gray-400 text-sm ml-1">by {{ article.username }}</span></h3>
              <p class="text-gray-600 text-sm">{{ truncateText(article.content) }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
<script>
import jwtDecode from 'jwt-decode';

  export default {
    data() {
      return {
        articles: [],
        comments: [],
        mainArticle: null,
        newComment: '',
        isAuthenticated: false,
        currentUser: ''
      };
    },
    async mounted() {
      try {
        const token = localStorage.getItem('token');
        if (token) {
          this.isAuthenticated = true;
          const decodedToken = jwtDecode(token);
          console.log(decodedToken);
          this.currentUser = "blm";
        }
        const response = await fetch('http://localhost:8080/article/all');
        this.articles = await response.json();
        this.articles.forEach(article => {
          if (!article.likes) {
            article.likes = 0;
          }
          if (!article.comments) {
          article.comments = [];
        }
        });
        this.mainArticle = this.articles[0];
        this.fetchComments(this.mainArticle.id);
      } catch (error) {
        console.error(error);
      }
    },
    methods: {
      truncateText(text, lim = 200) {
        return text.length > lim ? text.slice(0, lim) + '...' : text;
      },
      setMainArticle(article) {
        this.mainArticle = article;
        this.fetchComments(article.id);
      },
      likeArticle(article) {
        article.likes++;
      },
      async addComment(article) {
        if (this.newComment.trim() !== '') {
          try {
            const response = await fetch('http://localhost:8080/comment/create', {
              method: 'POST',
              headers: {
                'Content-Type': 'application/json',
                'Authorization': `Bearer ${localStorage.getItem('token')}`
              },
              body: JSON.stringify({
                article_id: article.id,
                content: this.newComment
              })
            })

            if (response.ok){
              const newComment = await response.json();
              this.comments.push(newComment);
              this.newComment = '';
            } else {
              console.log("error: ", response);
            }
          } catch (error) {
            console.log("error: ", error)
          }
        }
      },
      async fetchComments(articleId) {
        try {
          const response = await fetch(`http://localhost:8080/comment?article_id=${articleId}`);
          const comments = await response.json();
          this.comments = comments;
        } catch (error) {
          console.error(error);
          this.comments = [];
        }
      }
    }
  }
  </script>
  
  <style scoped>
      @import '~/assets/css/home.css';
  </style>
  