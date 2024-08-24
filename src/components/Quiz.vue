<template>
  <div class="quiz">
    <div v-if="testCompleted">
      <Results :questions="shuffledQuestions" @restart="restartQuiz" />
    </div>
    <div v-else-if="currentQuestion">
      <h1 class="quiz__title">Тестирование</h1>
      <div class="quiz__question question">
        <h2 class="question__title">{{ currentQuestion.question }}</h2>
        <ul class="question__body list-reset flex">
          <li
            v-for="answer in shuffledAnswers"
            :key="answer.text"
          >
            <label class="flex">
              <input
                type="radio"
                name="answer"
                :value="answer.text"
                @click="selectAnswer(answer, $event)"
                class="question__radio-button"
              />
              <span class="question__answer">{{ answer.text }}</span>
            </label>
          </li>
        </ul>
      </div>
      <ProgressBar :current="currentQuestionIndex + 1" :total="shuffledQuestions.length" />
    </div>
  </div>
</template>

<script setup>
  import { ref, computed } from 'vue';
  import ProgressBar from './ProgressBar.vue';
  import Results from './Results.vue';

  const props = defineProps({
    questions: {
      type: Array,
      required: true,
    },
  });

  function shuffleArray(array) {
    return array.sort(() => Math.random() - 0.5);
  }

  let shuffledQuestions = ref(shuffleArray([...props.questions]));
  const currentQuestionIndex = ref(0);

  const currentQuestion = computed(() => shuffledQuestions.value[currentQuestionIndex.value]);

  const shuffledAnswers = computed(() => {
    return shuffleArray([...currentQuestion.value.answers]);
  });

  const testCompleted = ref(false);
  let isTransitioning = ref(false);

  function selectAnswer(answer, event) {
    if (isTransitioning.value) {
      event.preventDefault();
      return;
    }

    if (event.detail === 0) {
      event.preventDefault();
      return;
    }

    shuffledQuestions.value[currentQuestionIndex.value].userAnswer = answer;
    isTransitioning.value = true;

    setTimeout(() => {
      if (currentQuestionIndex.value < shuffledQuestions.value.length - 1) {
        currentQuestionIndex.value++;
      } else {
        testCompleted.value = true;
      }
      isTransitioning.value = false;
    }, 1000);
  }

  function restartQuiz() {
    shuffledQuestions.value = shuffleArray([...props.questions]);
    currentQuestionIndex.value = 0;
    testCompleted.value = false;
    isTransitioning.value = false;
  }
</script>

<style scoped>
  .quiz__title {
    margin: 0;
    margin-bottom: 24px;
    font-weight: 800;
    font-size: 48px;
    letter-spacing: -0.6px;
  }

  .question {
    margin-bottom: 17px;
    height: 528px;
    padding: 40px;
    background: #f7f7f7;
    border-radius: 20px;
  }

  .question__title {
    margin: 0;
    margin-bottom: 20px;
    font-weight: 700;
    font-size: 27px;
    letter-spacing: -0.2px;
  }

  .question__body {
    flex-direction: column;
    gap: 20px;
  }

  .question__radio-button {
    position: relative;
    width: 20px;
    height: 20px;
    margin-right: 20px;
    border: 1px solid #000;
    border-radius: 50%;
    appearance: none;
    cursor: pointer;
    vertical-align: middle;
    flex-shrink: 0;
  }

  .question__radio-button:checked {
    background-color: #30f;
    border-color: #30f;
  }

  .question__radio-button:focus,
  .question__radio-button:hover {
    border-color: #30f;
    outline: none;
  }

  .question__answer {
    font-weight: 400;
    font-size: 17px;
    vertical-align: middle;
    letter-spacing: 0.55px;
  }
</style>
