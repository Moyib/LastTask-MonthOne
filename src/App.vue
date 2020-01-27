<template>
  <div id="app">
    <transition :duration="{ enter: 300, leave: 300 }" enter-active-class="animated zoomIn" leave-active-class="animated zoomOut" mode="out-in">

      <!-- The questions -->
      <div class="quizQuestions" v-if="questionIndex<quizData.questions.questions.length">
        <header>
          <h1 class="title quiztitle">How much of a patriot are you?</h1>
          <div class="progressContainer">
            <progress class="progress" :value="(questionIndex/quizData.questions.length)*100" max="100">
            {{ (questionIndex/quizData.questions.length)*100 }}%
            </progress>
            <p>
              {{ (questionIndex/quizData.questions.length)*100 }}% complete
            </p>
          </div>
        </header>

        <h2 class="questionTitle">
          {{ quizData.questions[questionIndex].text }}
        </h2>

        <div class="optionContent">
          <div class="option" v-for="(response, index) in quizData.questions[questionIndex].responses" @click="selectOption(index)" :class="{'is-selected' : userResponses[questionIndex] == index}" :key="index">
            {{ responses.option }}. {{ response.text }}
          </div>
        </div>

        <footer class="quizFooter">
          <nav class="pagination" role="navigation">
            <a class="button" @click="prev();" :disabled="questionIndex < 1">Go back</a>

            <a class="button" :class="(userResponses[questionIndex]==null)?'':'is-active'" @click="next();" :disabled="questionIndex>=quizDataa.questions.length"> {{ (userResponses[questionIndex]==null)?'Skip':'Next' }}</a>
          </nav>
        </footer>
      </div> <!-- End of the question content -->

      <!-- Quiz Results -->
      <div class="quizEnd" v-if="questionIndex >= quizData.questions.length" :key="questionIndex">
        
        <p class="scorecard">
          Total score is: {{ score() }} / {{ quizData.questions.length }}
        </p>
        <a class="button" @click="restart()">Take quiz again <i class="fa fa-refresh"></i></a>
      </div>
    </transition>
  </div>
</template>

<script>
import Vue from 'vue';
let i = 0;
const quizData = {
  title: "How well do you know Nigeria?",
  questions: [
   {
      text: "Where is the capital of Nigeria?",
      responses: [
        { option: "a", text: "Lagos" },
        { text: "Kaduna" },
        { text: "Abuja", correct: true },
        { text: "Plateau" }
      ]
   },

   {
      text: "What state was the first official capital of Nigeria?",
      responses: [
        { text: "Taraba" },
        { text: "Ondo" },
        { text: "Abuja" },
        { text: "Lagos", correct: true }
      ]
   },

    {
      text: "What name was Calabar originally called?",
      responses: [
        { text: "Akwaa Akpa", correct: true },
        { text: "Old Calabar" },
        { text: "Enwang" },
        { text: "Eniong" }
      ]
   },

   {
      text: "Approximately how many ethnic groups do we have in Nigeria?",
      responses: [
        { text: "About 500", correct: true },
        { text: "About 600" },
        { text: "About 700" },
        { text: "About 800" }
      ]
   },

   {
      text: "When was the first building in Nigeria built?",
      responses: [
        { text: "1843" },
        { text: "1844" },
        { text: "1845", correct: true },
        { text: "1846" }
      ]
   }
  ]
},

userResponsesContainer = Array(quizData.questions.length).fill(null);

export default {
  name: 'app',
  data() {
    return {
      quizData: quizData,
      questionIndex: 0,
      userResponses: userResponsesContainer,
      isActive: false
    }
  },

  filters: {
    respIndex() {
      return String.fromCharCode(97 + i);
    }
  },

  methods: {
    restart() {
      this.questionIndex=0;
      this.userResponses;
    },
    selectOption: function(index) {
      Vue.set(this.userResponses, this.questionIndex, index);
    },
    next() {
      if (this.questionIndex < this.quizData.questions.length)
      this.questionIndex++;
    },
    prev() {
      if (this.quizData.questions.length > 0)
      this.questionIndex--;
    },
    score() {
      var score = 0;
      for (let i = 0; i < this.userResponses.length; i++) {
        if (typeof this.quizData.questions[i].responses[this.userResponses[i]] !== "undefined" && this.quizData.questions[i].responses[this.userResponses[i]].correct) {
          score += 1;
        }
      }
      return score;
    }
  }
}
</script>

<style>
 #app {
   font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
   font-size: 14px;
   height: 100vh;
   background-color: #decade;
 }
</style>
