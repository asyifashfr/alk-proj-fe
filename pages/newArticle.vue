<template>
    <div class="container mx-auto px-4 py-8">
      <header class="text-2xl font-bold mb-8 text-center">
        Add New Article
      </header>
      <div class="mb-8 p-6 bg-white rounded-lg shadow-md">
        <form @submit.prevent="addArticle">
          <div class="mb-4">
            <label for="title" class="block text-gray-700">Title</label>
            <input
              v-model="newArticle.title"
              id="title"
              type="text"
              class="w-full p-2 border border-gray-300 rounded mt-1"
              required
            />
          </div>
          <div class="mb-4">
            <label for="content" class="block text-gray-700">Content</label>
            <textarea
              v-model="newArticle.content"
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
                Publish
            </button>
          </div>
        </form>
      </div>
    </div>
  </template>
  
  <script setup>
  const newArticle = ref({
    title: '',
    content: '',
  });
  
  const router = useRouter();
  
  const addArticle = () => {
    router.push('/managearticle');
  };
  </script>
  
  <style scoped>
  </style>
  