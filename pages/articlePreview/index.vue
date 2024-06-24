<template>
  <div class="mx-auto py-20" style="min-height: 100vh">
    <div class="flex flex-col lg:flex-row gap-8 px-4">
      <!-- Article Preview -->
      <div class="w-full lg:w-1/2 mb-20" v-if="articles && articles.length > 0">
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
              <p class="text-gray-800 font-semibold">{{ comment.username }}</p>
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
      <!-- All Articles -->
      <div class="lg:w-1/2 space-y-4 cursor-pointer max-h-maxh-screen">
        <header class="text-2xl text-white font-bold text-center rounded-md sticky top-0 bg-black z-10 p-4">
          Our Articles
        </header>
        <div class="flex mb-4 item-center space-x-4">
          <input 
            v-model="searchQuery"
            @input="filterArticles"
            type="text"
            placeholder="Search articles..."
            class="w-full p-2 border border-gray-300 rounded"
          />
          <p class="text-center"><strong>sort by :</strong></p>
          <select v-model="sortOrder" @change="sortArticles" class="ml-2 p-2 border border-gray-300 rounded">
            <option value="newest">Newest</option>
            <option value="latest">Latest</option>
            <option value="alphabetical">A-Z</option>
            <option value="likes">Most Liked</option>
          </select>
        </div>
        <div class="w-full space-y-4 cursor-pointer overflow-y-auto max-h-maxh-screen" v-if="filteredArticles && filteredArticles.length > 0">
          <div v-for="(article, index) in filteredArticles" :key="index" class="p-4 bg-white rounded-lg shadow-md hover:bg-gray-200" @click="setMainArticle(article)">
            <h3 class="text-md font-medium mb-1">{{ article.title }} <span class="text-gray-400 text-sm ml-1">by {{ article.username }}</span></h3>
            <p class="text-gray-600 text-sm">{{ truncateText(article.content) }}</p>            
          </div>
        </div>
        <div v-else class="text-gray-600 text-center">No articles found.</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      articles: [],
      filteredArticles: [],
      randomArticles: [],
      comments: [],
      mainArticle: null,
      newComment: '',
      isAuthenticated: false,
      currentUser: '',
      searchQuery: '',
      sortOrder: 'newest'
    };
  },
  async mounted() {
    const token = localStorage.getItem('token');
    if (token) {
      this.isAuthenticated = true;
    }

    const articleId = this.$route.query.articleId;
    console.log("Article ID from route:", articleId);

    try {
      const response = await fetch('http://localhost:8080/article/all');
      this.articles = await response.json();
      console.log("Articles fetched:", this.articles);
      this.articles.forEach(article => {
        if (!article.likes) {
          article.likes = 0;
        }
        if (!article.comments) {
          article.comments = [];
        }
      });
      this.filteredArticles = this.articles;
      if (articleId) {
        this.mainArticle = this.articles.find(article => article.id == articleId);
        console.log("Main article set:", this.mainArticle);
        if (this.mainArticle) {
          this.fetchComments(this.mainArticle.id);
        }
      } else {
        this.mainArticle = this.articles[0]
      }
    } catch (error) {
      console.error(error);
    }
  },
  methods: {
    truncateText(text, lim = 200) {
      return text.length > lim ? text.slice(0, lim) + '...' : text;
    },
    setMainArticle(article) {
      console.log("Setting main article:", article);
      this.mainArticle = article;
      this.fetchComments(article.id);
    },
    async likeArticle(article) {
      try {
        const response = await fetch('http://localhost:8080/article/like', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${localStorage.getItem('token')}`
          },
          body: JSON.stringify({
            article_id: article.id,
          })
        })

        if (response.ok){
          this.mainArticle.likes++;
        } else {
          console.log("Error liking article:", response);
        }
      } catch (error) {
        console.log("Error liking article:", error)
      }
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
            this.newComment = '';
            this.fetchComments(article.id);
          } else {
            console.log("Error adding comment:", response);
          }
        } catch (error) {
          console.log("Error adding comment:", error)
        }
      }
    },
    async fetchComments(articleId) {
      try {
        const response = await fetch(`http://localhost:8080/comment?article_id=${articleId}`);
        const comments = await response.json();
        this.comments = comments;
        console.log("Comments fetched:", this.comments);
      } catch (error) {
        console.error("Error fetching comments:", error);
        this.comments = [];
      }
    },
    filterArticles() {
      const query = this.searchQuery.toLowerCase();
      this.filteredArticles = this.articles.filter(article =>
        article.title.toLowerCase().includes(query) ||
        article.content.toLowerCase().includes(query)
      );
      this.sortArticles();
    },
    sortArticles() {
      if (this.sortOrder === 'alphabetical') {
        this.filteredArticles.sort((a, b) => a.title.localeCompare(b.title));
      } else if (this.sortOrder === 'likes') {
        this.filteredArticles.sort((a, b) => b.likes - a.likes);
      } else if (this.sortOrder === 'newest') {
        this.filteredArticles.sort((a, b) => b.id - a.id); 
      } else if (this.sortOrder === 'latest') {
        this.filteredArticles.sort((a, b) => a.id - b.id); 
      }
    },
    getRandomArticles(count) {
      let shuffled = [...this.articles].sort(() => 0.5 - Math.random());
      return shuffled.slice(0, count);
    }
  }
}
</script>

<style scoped>
  @import '~/assets/css/home.css';
</style>
