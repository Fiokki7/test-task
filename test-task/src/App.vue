<template>
  <div class="container">
    <Quiz :questions="questions" v-if="questions.length" />
    <div v-else>Загрузка...</div>
  </div>
</template>

<script setup>
  import { ref, onMounted } from 'vue';
  import Quiz from './components/Quiz.vue';
  import axios from 'axios';

  const questions = ref([]);

  onMounted(async () => {
    try {
      const response = await axios.get('http://localhost:3000/questions');
      questions.value = response.data;
    } catch (error) {
      console.error('Ошибка при загрузке данных:', error);
    }
  });
</script>

<style scoped>
  @import './assets/styles/normalize.css';
  @import './assets/styles/main.css';
</style>
