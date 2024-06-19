<template>
    <div class="container mx-auto px-4 py-8">
      <header class="text-2xl font-bold mb-8 text-center">
        Manage Articles
      </header>
      <!-- Articles List -->
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
        <div
          v-for="(article, index) in articles"
          :key="index"
          class="p-4 bg-white rounded-lg shadow-md"
        >
          <h3 class="text-lg font-medium mb-2">{{ article.title }}</h3>
          <p class="text-gray-600 mb-4">{{ truncateText(article.content) }}</p>
          <div class="flex justify-end space-x-2">
            <button
            @click="editArticle(index)"
            class="bg-gray-300 text-gray-800 p-2 rounded-md hover:bg-gray-400"
          >
            <i class="fas fa-edit"></i>
          </button>
          <button
            @click="deleteArticle(index)"
            class="bg-gray-300 text-gray-800 p-2 rounded-md hover:bg-gray-400"
          >
            <i class="fas fa-trash"></i>
          </button>
          </div>
        </div>
      </div>
      <!-- Floating "+" button -->
      <button
        @click="goToNewArticle"
        class="fixed bottom-8 right-10 bg-blue-500 text-white font-bold p-6 rounded-full shadow-lg hover:bg-blue-600 large-button"
      >
        Add New Article
      </button>
    </div>
  </template>
  
  <script setup>
  const articles = ref([
    {
      title: 'The Rise of JavaScript Frameworks and Their Impact on Web Development',
      content:
        'JavaScript frameworks have revolutionized web development, providing developers with powerful tools to build dynamic and responsive applications. In this article, we explore the most popular frameworks, their unique features, and how they are shaping the future of web development...',
    },
    {
      title: 'Exploring the Benefits of Serverless Architecture in Modern Applications',
      content:
        'Serverless architecture is gaining popularity as a way to streamline application development and deployment. In this article, we discuss the key benefits of serverless computing, including cost savings, scalability, and reduced maintenance overhead...',
    },
    {
      title: 'The Role of Artificial Intelligence in Enhancing User Experience',
      content:
        'Artificial Intelligence (AI) is transforming the way we interact with technology, offering new opportunities to enhance user experience. This article explores the various ways AI is being integrated into web and mobile applications...',
    },
  ]);
  
  const router = useRouter();
  
  const editArticle = (index) => {
    const article = articles.value[index];
    // TODO: edit article
  };
  
  const deleteArticle = (index) => {
    articles.value.splice(index, 1);
  };
  
  const truncateText = (text) => {
    const words = text.split(' ');
    return words.length > 20 ? words.slice(0, 20).join(' ') + '...' : text;
  };
  
  const goToNewArticle = () => {
    router.push('/newarticle');
  };
  </script>
  
  <style scoped>

  </style>
  