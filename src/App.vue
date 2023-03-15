<script setup>
import { ref,computed } from 'vue';

const questions = ref([
  {
    questions: 'whats is vue js ?',
    answer: 0,
    options:[
      'a javascript framework',
      'a javascript library',
      'a javascript compiler',
      'a javascript compiler',
    ],
    selected:null,
  },
  {
questions: 'whats is laravel ?',
answer: 0,
options:[
  'a php framework',
  'a php library',
  'a php compiler',
  'a php compiler',
],
selected:null,
},
{
questions: 'whats is php ?',
answer: 0,
options:[
  'a server side scripting language',
  'a client side scripting language',
  'a compiler',
  'a compiler',
],
selected:null,
},
{
questions: 'whats is mysql ?',
answer: 0,
options:[
  'a database',
  'a library',
  'a compiler',
  'a compiler',
],
selected:null,
  }
]);

const quizCompleted =ref(false);
const quizStarted = ref(false);
const CurrentQuistion = ref(0);
const progress = ref(0);
const start = ref(1);
const quiz = ref(0);
const end = ref(0);


const score = computed (()=>{
let value = 0 
questions.value.map( q=>{
  if(q.selected == q.answer){
    value++
  }
})
return value ;
});

const getCurrentQuestion = computed(()=>{
  quiz.value = 1;
  let question =questions.value[CurrentQuistion.value]
  question.index = CurrentQuistion.value
  return question ;
})

const SetAnswer = evt => {
  questions.value[CurrentQuistion.value].selected = evt.target.value
  evt.target.value = null
  progress.value = Math.ceil((CurrentQuistion.value + 1) / questions.value.length * 100)

}

const NextQuestion = () => {

  if (CurrentQuistion.value < questions.value.length - 1){
    CurrentQuistion.value ++
  }else{
    quizCompleted.value = true ;
  }
}

const countCorrectAnswers = () => {
  let correctAnswers = 0;
  questions.value.forEach((question) => {
    if (question.selected == question.answer) {
      correctAnswers++;
    }
  });
  return correctAnswers;
};

const countWrongAnswers = () => {
  end.value = 1;
  let wrongAnswers = 0;
  questions.value.forEach((question) => {
    if (question.selected != question.answer) {
      wrongAnswers++;
    }
  });
  return wrongAnswers;
 
};

</script>

<template>
    <header>
      <img src="../images/icons8-php-logo-160.png" alt="">
        <h1>PHP QUIZ TEST</h1>
    </header>

<main class="app">
<div class="landing" v-if="!quizStarted"> 
  
  <section class="container1">
            <h2>About</h2>
            <p>hello ,dear visiter this Quiz will test your php knowlege it will cover all the parts from (varibles,
                loops ,condotions...) as well as object orianted programing i hope youll enjoy it and if you are not
                ready i really advice you to take a look at our tutorial site, thank you .</p>
        </section>

        <div>
            <button class="example_g" type="button"  @click="quizStarted = true">Click to Start!</button>
            <img src="../images/Customer Survey-rafiki.svg" alt="">
        </div>

        <section class="container2">
            <h2>game rules</h2>
            <ul>
                <li>you have to anser all the quistions</li>
                <li>if you pass 30s your anser will count fals</li>
                <li>you may have one or more than one right anser </li>
                <li>the resault at the end </li>
            </ul>
    </section>
  </div>

  <section class="quiz" v-bind:disabled= "quizStarted" v-if="quizStarted && !quizCompleted" >
    <div class="progress-bar">
        <div class="progress" :style="{ width: `${progress}%` }"></div>
      </div>
      <div class="quiz-info">
        <h2>{{ getCurrentQuestion.questions }}</h2>
       <span class="score">score {{ score }}/ {{ questions.length }}</span>
      </div>
      <div class="options">
        <label
         v-for="(option, index) in getCurrentQuestion.options " 
        :key="index" 
        :class="`option ${ 
          getCurrentQuestion.selected == index 
            ? index == getCurrentQuestion.answer
                    ? 'correct'
                    : 'wrong'
            : ''
         } ${
            getCurrentQuestion.selected != null && 
            index != getCurrentQuestion.selected
            ? 'disabled'
            : ''
         }`">
          <input type="radio" 
          :name="getCurrentQuestion.index"
          :value="index"
          v-model = "getCurrentQuestion.selected"
          :disabled="getCurrentQuestion.selected"
          @change="SetAnswer">
          <span>{{ option }}</span>
        </label>
      </div>
      <button
      @click="NextQuestion" 
      :disabled="! getCurrentQuestion.selected">
        {{ 
        getCurrentQuestion.index == questions.length - 1 
          ? 'Finish'
          : getCurrentQuestion.selected == null
                ? 'select an option'
                : 'next question'
                }}
      </button>
  </section>
  <section v-if="quizCompleted">
    <h3>you have finished the quiz </h3>
    <h3>your score : {{  score }}/ {{ questions.length }} </h3>
    <h3> correct answers : {{ countCorrectAnswers() }}</h3>
    <h3>wrong answers : {{ countWrongAnswers() }} </h3>
  </section>

</main>
<div class="row">
    <div class="col-xs-12 col-md-8 offset-md-2 block border">
        <div class="wrapper-progressBar">
            <ul class="progressBar">
                <li :class="{ active: start === 1 }">Informations</li>
                <li :class="{ active: quiz === 1 }">Quistions</li>
                <li :class="{ active: end === 1 }">Resault</li>
            </ul>
        </div>
    </div>
</div>
</template>

<style>

.progress-bar {
  width: 100%;
  height: 20px;
  background-color: #ddd;
  border-radius: 10px;
  position: relative;
}

.progress {
  height: 100%;
  background-color: #2cce7d;
  border-radius: 10px;
  position: absolute;
  top: 0;
  left: 0;
  width: 0%;
  transition: width 0.5s ease;
}

body {
  display: grid;
      align-items: center;
      color: white;
      height: 100vh;
      margin: 0px;
      padding: 0px;
      background-image: url("/images/stacked-steps-haikei.png");
      background-size: cover;
  font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
}
.app{
  display: flex ;
  flex-direction: column;
  align-items: center;
  padding: 2rem;

}
h1{
  font-size: 2rem;
  margin-bottom: 2rem;
  
}
h2{
  font-size: 2rem;
  margin-bottom: 2rem;
  
}
.quiz {
  padding: 1rem;
  width: 100%;
  max-width: 1000px ;
  border-radius: 0.5rem;
}
.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem ;
}


.quiz-info .score {
  color : #fff;
  font-size: 1.25rem;
  font-weight: 700;
display: flex;
align-items: center;
}
.options{
 margin-bottom: 1rem;
}
.option{

  display: block;
  padding: 1rem;
  background-color: #00000029 ;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;

}
.option:hover{
  background-color: #ef9696;
}
.option.correct{
  background-color: #2cce7d;
}
.option.wrong{
  background-color: #fc0008;
}
.option:last-of-type {
  margin-bottom: 0 ;
}
.option.disabled{
  fill-opacity: 0.5;
}

</style>


  
  