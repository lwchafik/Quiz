<script setup>
import { shuffleArray } from '@/functions/Array';
import { computed, onMounted, onUnmounted, ref, watch } from 'vue';
import Answers from './Answers.vue';

  const props = defineProps({
    question: Object
  })

  const answer = ref(null)
  const emits = defineEmits(['answer'])
  const hasAnswer = computed(() => answer.value !== null)

  const randomChoices = computed(() => shuffleArray(props.question.choices))

  let timer

  const onAnswer = (e) => {
    clearTimeout(timer)
    timer = setTimeout(() => {
      emits('answer', answer.value)
    }, 1000)
  }

  onMounted(() => {
    timer = setTimeout(() => {
      answer.value = ''
      onAnswer()
    }, 5000)
  })

  onUnmounted(() => {
    clearTimeout(timer)
  })

  // watch(() => props.question, () => {
  //   answer.value = null
  // })  this code is responsible of clearing the answer every time we move to the next question. This code is replaced by :key='question.question' in the question tag in Quiz.vue file


</script>

<template>
  <div class="question">
    <h3>{{ question.question }}</h3>
    <div v-for="(choice, index) in randomChoices" :key="choice">
      <Answers :disabled="hasAnswer" :id="`answer${index}`" v-model="answer" :value="choice" :correctAnswer="question.correct_answer" @change="onAnswer" />
    </div>
    <!-- <button :disabled="!hasAnswer" @click="emits('answer', answer)">Next</button> -->
  </div>
</template>

<style>
  .question {
    padding-top: 2rem;
  }
  .question button {
    margin-left: auto;
    display: block;
  }
</style>