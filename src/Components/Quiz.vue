<script setup>
  import { computed, ref } from 'vue';
  import Progress from './Progress.vue';
  import Question from './Question.vue';
  import Recap from './Recap.vue';

  const props = defineProps({
    quiz: Object
  })

  const step = ref(0)
  const question = computed(() => props.quiz.questions[step.value])

  const state = ref('question')
  const answers = ref(props.quiz.questions.map(() => null))
  const addAnswer = (answer) => {
    answers.value[step.value] = answer
    if (step.value === props.quiz.questions.length - 1) {
      state.value = 'recap'
    } else {
      step.value++
    }    
  }
</script>

<template>
  <div>
    <h1>{{ quiz.title }}</h1>
    <Progress :value="step" :max="quiz.questions.length - 1" />
    <Question :key="question.question" :question="question" v-if="state === 'question'" @answer="addAnswer"/>
    <Recap v-if="state === 'recap'" :answers="answers" :quiz="quiz"/>
  </div>
</template>