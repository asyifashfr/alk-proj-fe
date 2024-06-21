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
          <!-- <div id="quill-editor" class="w-full p-2 border border-gray-300 rounded mt-1"></div> -->
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
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import Quill from 'quill';
import 'quill/dist/quill.snow.css';

const newArticle = ref({
  title: '',
  content: ''
});

const router = useRouter();
let quill;

onMounted(() => {
  quill = new Quill('#quill-editor', {
    theme: 'snow',
    placeholder: 'Compose an epic...',
    modules: {
      toolbar: [
        [{ header: [1, 2, false] }],
        ['bold', 'italic', 'underline'],
        ['link', 'blockquote', 'code-block', 'image'],
        [{ list: 'ordered' }, { list: 'bullet' }]
      ]
    }
  });

  quill.on('text-change', () => {
    newArticle.value.content = quill.root.innerHTML;
  });
});

const addArticle = async () => {
  const token = localStorage.getItem('token');
  if (!token) {
    alert('You must be logged in to post an article.');
    return;
  }

  try {
    const response = await fetch('http://localhost:8080/article/create', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`
      },
      body: JSON.stringify({
        title: newArticle.value.title,
        content: newArticle.value.content,
      })
    });

    if (!response.ok) {
      throw new Error('Failed to create article');
    }

    alert('Article published successfully!');

    router.push('/manageArticle');
  } catch (error) {
    console.error('Error publishing article:', error);
    alert('Error publishing article: ' + error.message);
  }
};
</script>

<style scoped>
@import 'quill/dist/quill.snow.css';

#quill-editor {
  height: 200px; /* Set the desired height for the editor */
}
</style>
