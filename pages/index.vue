<template>
    <div class="container mx-auto px-4 py-8">
      <header class="text-lg font-bold mb-8 text-center">
        Our Articles
      </header>
      <div class="flex flex-col lg:flex-row gap-8">
        <!-- Latest Article Preview -->
        <div class="w-full lg:w-3/5">
          <div class="p-6 bg-white rounded-lg shadow-md">
            <h2 class="text-lg font-semibold mb-4 text-center">Understanding the Complexities of Modern Web Development</h2>
            <p class="text-gray-700 mb-4">
              Lorem ipsum dolor sit amet, consectetur adipiscing elit. Suspendisse suscipit quam sed sapien venenatis egestas. Vestibulum accumsan, urna eget commodo tempor, nulla libero mattis nibh, vel auctor dui orci ac risus. Aliquam vitae orci sit amet dolor dignissim dignissim sed a nibh. Curabitur venenatis imperdiet arcu, eu sagittis urna. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Nunc interdum nisi aliquam tortor accumsan, ut tristique dui scelerisque. Quisque iaculis nunc augue, id posuere ex rutrum ac. Nunc vulputate faucibus facilisis.
            </p>
            <p class="text-gray-700 mb-4">
              Pellentesque fermentum gravida velit in interdum. Integer sed sodales nulla. Nulla facilisi. Integer lacus felis, dignissim eget ligula eget, rutrum feugiat eros. Duis non placerat sem, eu gravida nunc. Proin nisi mi, ultrices vel tincidunt in, commodo ac lectus. Nunc nec eros libero...
            </p>
            <button class="text-blue-500 hover:underline">Read More</button>
          </div>
        </div>
        <!-- Other Articles -->
        <div class="w-full lg:w-2/5 space-y-4">
          <div class="p-4 bg-white rounded-lg shadow-md">
            <h3 class="text-md font-medium mb-1">The Rise of JavaScript Frameworks and Their Impact on Web Development</h3>
            <p class="text-gray-600 text-sm">{{ truncateText(otherArticles[0].content) }}</p>
          </div>
          <div class="p-4 bg-white rounded-lg shadow-md">
            <h3 class="text-md font-medium mb-1">Exploring the Benefits of Serverless Architecture in Modern Applications</h3>
            <p class="text-gray-600 text-sm">{{ truncateText(otherArticles[1].content) }}</p>
          </div>
          <div class="p-4 bg-white rounded-lg shadow-md">
            <h3 class="text-md font-medium mb-1">The Role of Artificial Intelligence in Enhancing User Experience</h3>
            <p class="text-gray-600 text-sm">{{ truncateText(otherArticles[2].content) }}</p>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  const otherArticles = ref([
    {
      title: 'The Rise of JavaScript Frameworks and Their Impact on Web Development',
      content: 'JavaScript frameworks have revolutionized web development, providing developers with powerful tools to build dynamic and responsive applications. In this article, we explore the most popular frameworks, their unique features, and how they are shaping the future of web development. From the early days of jQuery to the modern ecosystems of React, Vue, and Angular, we delve into the evolution of JavaScript frameworks and what they mean for developers today. Understanding these tools is crucial for anyone looking to stay ahead in the rapidly changing tech landscape. Whether you\'re a seasoned developer or just starting, this comprehensive guide will provide valuable insights and practical tips to help you navigate the world of JavaScript frameworks. Join us as we uncover the trends, challenges, and opportunities that lie ahead in the world of web development.'
    },
    {
      title: 'Exploring the Benefits of Serverless Architecture in Modern Applications',
      content: 'Serverless architecture is gaining popularity as a way to streamline application development and deployment. In this article, we discuss the key benefits of serverless computing, including cost savings, scalability, and reduced maintenance overhead. By leveraging cloud services, developers can focus more on writing code and less on managing infrastructure. We also examine some of the leading serverless platforms, such as AWS Lambda, Azure Functions, and Google Cloud Functions, and provide practical examples of how to implement serverless solutions in real-world projects. Discover how serverless architecture can transform your development workflow and help you build efficient, scalable, and resilient applications.'
    },
    {
      title: 'The Role of Artificial Intelligence in Enhancing User Experience',
      content: 'Artificial Intelligence (AI) is transforming the way we interact with technology, offering new opportunities to enhance user experience. This article explores the various ways AI is being integrated into web and mobile applications to create more personalized, intuitive, and efficient user interfaces. From chatbots and virtual assistants to recommendation engines and predictive analytics, AI is enabling developers to build smarter applications that can anticipate and respond to user needs. We also discuss the ethical considerations and challenges associated with AI in UX design, and how to ensure that AI-driven solutions are both effective and user-friendly. Learn how to harness the power of AI to deliver superior user experiences and stay competitive in the digital age.'
    }
  ]);
  
  const truncateText = (text) => {
    const words = text.split(' ');
    return words.length > 20 ? words.slice(0, 20).join(' ') + '...' : text;
  };
  </script>
  
  <style lang="scss" scoped>
  
  </style>
  