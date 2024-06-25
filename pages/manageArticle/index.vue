<template>
  <div class="container mx-auto px-4 mt-20">
    <header class="text-2xl font-bold mb-8 text-center">
      Manage My Articles
    </header>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8" v-if="articles && articles.length > 0">
      <div
        v-for="(article, index) in articles"
        :key="index"
        class="p-4 bg-white rounded-lg shadow-md flex flex-col justify-between"
      >
        <div class="w-full h-28 flex-shrink-0 mr-4 bg-gray-200 mb-4">
          <NuxtImg :src=getImageUrl(article.photo_url) alt="Article Image" class="w-full h-full object-cover rounded-md" />
        </div>
        <div>
          <h3 class="text-lg font-medium mb-2">{{ article.title }}</h3>
          <p class="text-gray-600 mb-4">{{ truncateText(article.content) }}</p>
        </div>
        <div class="flex justify-end space-x-2">
          <button @click="editArticle(index)" class="bg-gray-300 text-gray-800 p-2 rounded-md hover:bg-gray-400">
            <i class="fas fa-edit"></i>
          </button>
          <button @click="confirmDeleteArticle(index)" class="bg-gray-300 text-gray-800 p-2 rounded-md hover:bg-gray-400">
            <i class="fas fa-trash"></i>
          </button>
        </div>
      </div>
    </div>
    <div v-else class="text-center text-gray-600">
      No articles found
    </div>
    <button @click="goToNewArticle" class="fixed bottom-8 right-10 w-48 bg-gradient-to-r from-yellow-300 to-yellow-600 text-white font-bold p-6 rounded-full text-lg shadow-lg hover:bg-yellow-600">
      Create Article
    </button>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

const articles = ref([]);

const router = useRouter();

const getImageUrl = (photoUrl) => {
  return photoUrl ? `http://localhost:8080${photoUrl}` : 'https://via.placeholder.com/150';
};

onMounted(async () => {
const token = localStorage.getItem('token');
if (!token) {
  console.error('No token found');
  router.push('/login');
  return;
}
try {
  const response = await fetch('http://localhost:8080/article/my', {
    method: 'GET',
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json'
    }
  });
  if (!response.ok) throw new Error('Failed to fetch articles');
  articles.value = await response.json();
} catch (error) {
  console.error('Error fetching articles:', error.message);
}
});

const editArticle = (index) => {
  router.push(`/editArticle/${articles.value[index].id}`);
console.log('Editing article:', articles.value[index]);
};

const confirmDeleteArticle = (index) => {
  const confirmed = window.confirm('Are you sure you want to delete this article?');
  if (confirmed) {
    deleteArticle(index);
  }
};

const deleteArticle = async (index) => {
  const articleId = articles.value[index].id;
  try {
    const token = localStorage.getItem('token');
    const response = await fetch(`http://localhost:8080/article/delete?id=${articleId}`, {
      method: 'DELETE',
      headers: {
        'Authorization': `Bearer ${token}`,
        'Content-Type': 'application/json'
      }
    });
    if (!response.ok) throw new Error('Failed to delete article');
    articles.value.splice(index, 1);
  } catch (error) {
    console.error('Error deleting article:', error.message);
  }
};

const truncateText = (text) => {
const words = text.split(' ');
return words.length > 20 ? words.slice(0, 20).join(' ') + '...' : text;
};

const goToNewArticle = () => {
router.push('/newArticle');
};
</script>

<style scoped>
</style>
