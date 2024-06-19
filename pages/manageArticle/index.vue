<template>
  <div class="container mx-auto px-4 py-8">
    <header class="text-2xl font-bold mb-8 text-center">
      Manage My Articles
    </header>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8" v-if="articles && articles.length > 0">
      <div
        v-for="(article, index) in articles"
        :key="index"
        class="p-4 bg-white rounded-lg shadow-md"
      >
        <h3 class="text-lg font-medium mb-2">{{ article.title }}</h3>
        <p class="text-gray-600 mb-4">{{ truncateText(article.content) }}</p>
        <div class="flex justify-end space-x-2">
          <button @click="editArticle(index)" class="bg-gray-300 text-gray-800 p-2 rounded-md hover:bg-gray-400">
            <i class="fas fa-edit"></i>
          </button>
          <button @click="deleteArticle(index)" class="bg-gray-300 text-gray-800 p-2 rounded-md hover:bg-gray-400">
            <i class="fas fa-trash"></i>
          </button>
        </div>
      </div>
    </div>
    <div v-else class="text-center text-gray-600">
      No articles found
    </div>
    <button @click="goToNewArticle" class="fixed bottom-8 right-10 bg-blue-500 text-white font-bold p-6 rounded-full shadow-lg hover:bg-blue-600 large-button">
      Add New Article
    </button>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

const articles = ref([]);

const router = useRouter();

onMounted(async () => {
const token = localStorage.getItem('token');
if (!token) {
  console.error('No token found');
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
