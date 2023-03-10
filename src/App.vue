<script setup>

import { ref } from 'vue'
const question = ref([
  {
    question: 'What is the capital of India?',
    answer:0,
    options: [
      'New Delhi',
      'Mumbai',
      'Kolkata',
      'Chennai'
    ],
    selected: null
  },
  {
    question: 'What is the capital of Pakistan?',
    answer:2,
    options: [
      'Islamabad',
      'Karachi',
      'Lahore',
      'Peshawar'
    ],
    selected: null
  },
  {
    question: 'What is the capital of morocco?',
    answer:2,
    options: [
      'Rabat',
      'Casablanca',
      'Tangier',
      'Marrakech'
    ],
    selected: null
  }

])

const quizComplete = ref(false)
const correctQuestion = ref(0)
const score = computed(()=> {
 let value = 0
question.value.map(q => {
  if (q.selected === q.answer) {
    value++
  }
})
return value
})

  const getCurrentQuestion = computed(() => {
    let question = question.value[correctQuestion.value]
    question.index = correctQuestion.value
    return question
  })

  const setAnswer = evt => {
    question.value[correctQuestion.value].selected = evt.target.value
    evt.target.value = null
  }
  const nextQuestion = () => {
    if (correctQuestion.value < question.value.length - 1) {
      correctQuestion.value++
    } else {
      quizComplete.value = true
    }
  }
</script>

<template>
    <main class="app">
    <h1>Quiz App</h1>
    <section class="quiz">
      <div class="quiz-info">
      <span class="question">{{getCurrentQuestion.question}}</span>
      <span class="score">Score: {{score}}/{{question.length}}</span>
      </div>

      <div class="options">
        <button v-for="(option, index) in getCurrentQuestion.options" :key="index" @click="setAnswer" :value="index">{{option}}</button>
      </div>
    </section>
    </main>
</template>

<style>

  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body{
    background-color: #fadddd;
    color: rgb(97, 167, 227);
  }
</style>
