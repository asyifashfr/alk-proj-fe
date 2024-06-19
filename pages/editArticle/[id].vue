<template>
    <div class="container mx-auto px-4 py-8">
      <header class="text-2xl font-bold mb-8 text-center">
        Edit Article
      </header>
      <div class="mb-8 p-6 bg-white rounded-lg shadow-md">
        <form @submit.prevent="updateArticle">
          <div class="mb-4">
            <label for="title" class="block text-gray-700">Title</label>
            <input
              v-model="article.title"
              id="title"
              type="text"
              class="w-full p-2 border border-gray-300 rounded mt-1"
              required
            />
          </div>
          <div class="mb-4">
            <label for="content" class="block text-gray-700">Content</label>
            <textarea
              v-model="article.content"
              id="content"
              rows="4"
              class="w-full p-2 border border-gray-300 rounded mt-1"
              required
            ></textarea>
          </div>
          <div class="flex justify-end">
            <button
              type="submit"
              class="bg-blue-500 text-white py-2 px-4 rounded hover:bg-blue-600"
            >
              Update Article
            </button>
          </div>
        </form>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  import { useRoute, useRouter } from 'vue-router';
  
  const article = ref({
    title: '',
    content: '',
    user_id: ''
  });
  
  const router = useRouter();
  const route = useRoute();
  
  onMounted(async () => {
    const articleId = route.params.id;
    try {
      const token = localStorage.getItem('token');
      const response = await fetch(`http://localhost:8080/article?id=${articleId}`, {
        method: 'GET',
        headers: {
          'Authorization': `Bearer ${token}`,
          'Content-Type': 'application/json'
        }
      });
  
      if (!response.ok) {
        throw new Error('Failed to fetch article');
      }
  
      const data = await response.json();
      article.value = data;
      console.log('Fetched article:', article.value);
    } catch (error) {
      console.error('Error fetching article:', error);
    }
  });
  
  const updateArticle = async () => {
    const articleId = route.params.id;
    const token = localStorage.getItem('token');

    try {
        const id = parseInt(articleId); 
        const response = await fetch(`http://localhost:8080/article/edit`, {
            method: 'PUT',
            headers: {
                'Authorization': `Bearer ${token}`,
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                id: id,
                title: article.value.title,
                content: article.value.content,
                user_id: article.value.user_id
            })
        });

        console.log('Response:', response)
        console.log("request", JSON.stringify({
            id: id,
            title: article.value.title,
            content: article.value.content,
            user_id: article.value.user_id
        }))
        if (!response.ok) {
            throw new Error('Failed to update article');
        }

        alert('Article updated successfully!');
        router.push('/manageArticle'); 
    } catch (error) {
        console.error('Error updating article:', error);
        alert('Error updating article: ' + error.message);
    }
  };
  </script>
  
  <style scoped>
  </style>
  