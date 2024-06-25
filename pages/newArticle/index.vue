<template>
  <div class="container mx-auto px-4 py-8">
    <header class="text-2xl font-bold mb-8 text-center">
      Add New Article
    </header>
    <div class="mb-8 p-6 bg-white rounded-lg shadow-md">
      <form @submit.prevent="addArticle">
        <div class="mb-4">
          <div v-if="newArticle.image" class="flex justify-center mb-8">
            <img :src="imageUrl" alt="Selected Image" class="max-w-xs rounded-md" style="max-height: 256px; width: auto;">
          </div>
          <div class="flex flex-col items-center">
            <!-- <span v-if="newArticle.image" class="ml-2 mb-4">{{ newArticle.image.name }}</span> -->
            <input
              type="file"
              id="image"
              accept="image/*"
              @change="onImageChange"
              class="hidden"
            />
            <label
              for="image"
              class="text-black cursor-pointer w-16 h-16 flex items-center justify-center bg-white border border-black rounded-full hover:bg-gray-100"
            >
              <i class="fas fa-camera text-xl"></i>
            </label>
            <span class="block text-gray-700 mt-2">Upload Image</span>
          </div>
        </div>
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
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

const newArticle = ref({
  title: '',
  content: '',
  image: null
});

const imageUrl = ref(null);

const router = useRouter();

const onImageChange = (event) => {
  const file = event.target.files[0];
  if (file && file.type.startsWith('image/')) {
    newArticle.value.image = file;
    imageUrl.value = URL.createObjectURL(file);
  } else {
    alert('Please select a valid image file.');
  }
};

const addArticle = async () => {
  const token = localStorage.getItem('token');
  if (newArticle.title === '' || newArticle.content === '' || imageUrl === null) {
    alert('Please fill in all fields.');
    return;
  }

  if (!token) {
    alert('You must be logged in to post an article.');
    return;
  }

  try {
    let imageUrl = '';

    if (newArticle.value.image) {
      const formData = new FormData();
      formData.append('image', newArticle.value.image);

      const imgResponse = await fetch('http://localhost:8080/article/create/img', {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${token}`
        },
        body: formData
      })
  
      if (!imgResponse.ok) {
        throw new Error('Failed to upload image');
      }
  
      const imgData = await imgResponse.json();
      imageUrl = imgData.photo_url;
      console.log('Image uploaded:', imageUrl);
    }

    const response = await fetch('http://localhost:8080/article/create', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`
      },
      body: JSON.stringify({
        title: newArticle.value.title,
        content: newArticle.value.content,
        photo_url: imageUrl
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

</style>
