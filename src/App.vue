<script setup>
import { onMounted, ref } from "vue";
import Quiz from "./Components/Quiz.vue";

const quiz = ref(null);
const state = ref("loading");

onMounted(() => {
  fetch("/quiz.json")
    .then((r) => {
      if (r.ok) {
        return r.json();
      }
      throw new Error("Impossible to fetch the json");
    })
    .then((data) => {
      quiz.value = data;
      state.value = "idle";
    })
    .catch((e) => {
      state.value = "error";
    });
});
</script>

<template>
  <div class="container">
    <div v-if="state === 'error'">
      <p>Impossible to load the quiz</p>
    </div>
    <div :aria-busy="state === 'loading'">
      <Quiz :quiz="quiz" v-if="quiz" />
    </div>
  </div>
</template>

<style>
  .container {
    margin-top: 2rem;
  }
</style>