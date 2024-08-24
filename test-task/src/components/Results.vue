<template>
    <div class="results" aria-live="polite">
      <h2 class="results__title" v-if="correctAnswersCount === totalQuestions">Поздравляем!</h2>
      <h2 class="results__title" v-else-if="correctAnswersCount > 0">Хороший результат!</h2>
      <h2 class="results__title" v-else>Упс :(</h2>

      <p class="results__text" v-if="correctAnswersCount === totalQuestions">
      Вы правильно ответили на все вопросы.<br>Вы действительно отлично разбираетесь в IT.
      </p>
      <p class="results__text" v-else-if="correctAnswersCount > 0">
      Вы ответили правильно на {{ correctAnswersCount }} вопросов.<br>Так держать!
      </p>
      <p class="results__text" v-else>
      Вы неправильно ответили на все вопросы.<br>Нужно подучить теорию.
      </p>

      <ul class="results__list list-reset flex">
        <li
          v-for="(question, index) in questions"
          :key="index"
          :class="{'results__correct': question.userAnswer?.correct, 'results__incorrect': !question.userAnswer?.correct}"
        >
          <p class="results__question">{{ question.question }}</p>
          <p class="results__answer">
            {{ question.userAnswer?.text || 'Не отвечено' }}
          </p>
        </li>
      </ul>

      <button class="results__button" v-if="correctAnswersCount < totalQuestions" @click="restartQuiz">
        Пройти еще раз
      </button>
    </div>
  </template>

  <script setup>
    import { computed } from 'vue';

    const props = defineProps({
      questions: {
        type: Array,
        required: true,
      },
    });

    const emit = defineEmits(['restart']);

    const correctAnswersCount = computed(() => {
      return props.questions.filter(q => q.userAnswer?.correct).length;
    });

    const totalQuestions = computed(() => props.questions.length);

    function restartQuiz() {
      emit('restart');
    }
  </script>

  <style scoped>
    .results__title {
      margin: 0;
      margin-bottom: 26px;
      font-weight: 800;
      font-size: 47px;
    }

    .results__text{
      margin: 0;
      margin-bottom: 40px;
      font-weight: 400;
      font-size: 27px;
      line-height: 125%;
    }

    .results__list {
      margin-bottom: 40px;
      flex-direction: column;
      gap: 20px;
    }

    .results__correct,
    .results__incorrect {
      padding: 40px;
      border-radius: 20px;
      transition: background-color 0.3s ease;
    }

    .results__correct {
      background: #e5f5e1;
    }

    .results__incorrect {
      background: #ffe0e0;
    }

    .results__question {
      margin: 0;
      margin-bottom: 17px;
      font-weight: 700;
      font-size: 22px;
      letter-spacing: 0.1px;
    }

    .results__answer{
      margin: 0;
      font-weight: 400;
      font-size: 18px;
      line-height: 125%;
    }

    .results__button {
      display: block;
      margin: 0 auto;
      padding: 16px 30px;
      border-radius: 10px;
      background: #30f;
      font-weight: 700;
      font-size: 18px;
      color: #fff;
    }
  </style>
