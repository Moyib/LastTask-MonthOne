<template>

  <div class="main-body">
    <div id="app" class="quiz-body">
      <div class="intro">
        <h2>How much of a patriot are you?</h2>
        <a role="button" class="takeQuiz" @click="showQuiz = true">Take Quiz</a>
        <questions v-if="showQuiz" class="questions"></questions>

        <!-- Quiz Results -->
        <div class="quizEnd" v-if="questionIndex >= quizData.questions.length" :key="questionIndex">

          <p class="scorecard">
            Total score is: {{ score() }} / {{ quizData.questions.length }}
          </p>
          <a class="button" @click="restart">Take quiz again <i class="fa fa-refresh"></i></a>
        </div>
        
      </div>
    </div>
  </div>
</template>

<script>
import questions from './components/questions.vue';

export default {
  name: 'app',
  components: {
    questions,
  },

  data() {
    return {
      quizData: {
        title: "How well do you know Nigeria?",
        questions: [
          {
            text: "Where is the capital of Nigeria?",
            responses: [
              { option: 'a', text: "Lagos" },
              { option: 'b', text: "Kaduna" },
              { option: 'c', text: "Abuja", correct: true },
              { option: 'd', text: "Plateau" }
            ]
          },

          {
            text: "What state was the first official capital of Nigeria?",
            responses: [
              { option: 'a', text: "Taraba" },
              { option: 'b', text: "Ondo" },
              { option: 'c', text: "Abuja" },
              { option: 'd', text: "Lagos", correct: true }
            ]
          },

          {
            text: "What name was Calabar originally called?",
            responses: [
              { option: 'a', text: "Akwaa Akpa", correct: true },
              { option: 'b', text: "Old Calabar" },
              { option: 'c', text: "Enwang" },
              { option: 'd', text: "Eniong" }
            ]
          },

          {
            text: "Approximately how many ethnic groups do we have in Nigeria?",
            responses: [
              { option: 'a', text: "About 500", correct: true },
              { option: 'b', text: "About 600" },
              { option: 'c', text: "About 700" },
              { option: 'd', text: "About 800" }
            ]
          },

          {
            text: "When was the first building in Nigeria built?",
            responses: [
              { option: 'a', text: "1843" },
              { option: 'b', text: "1844" },
              { option: 'c', text: "1845", correct: true },
              { option: 'd', text: "1846" }
            ]
          }
        ]
      },
      questionIndex: 0,
      showQuiz: false,
      userResponses: [],
      isActive: false
    }
  },

  methods: {
    restart() {
      this.questionIndex = 0;
      this.userResponses = [];
    },
    selectOption: function(index) {
      this.$set(this.userResponses, this.questionIndex, index);
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
      let score = 0;
      for (let i = 0; i < this.userResponses.length; i++) {
        if (typeof this.quizData.questions[i].responses[this.userResponses[i]] !== "undefined" &&
            this.quizData.questions[i].responses[this.userResponses[i]].correct) {
          score += 1;
        }
      }
      return score;
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Ubuntu:300,400&display=swap');

.main-body {
  background-color: azure;
  position: fixed;
  height: 100vh;
  width: 100vw;
  font-family: 'Ubuntu', sans-serif;
  font-weight: 300;
}

.intro {
  position: relative;
  border-bottom: 1px solid #C3D1F0;
  padding: 5%;
  
}

.intro a.takeQuiz {
  padding: .5rem;
  border: 1px solid gainsboro;
  border-radius: .5rem;
  cursor: pointer;
  transition: 0.3s;
}
.intro a.takeQuiz:hover {
  background-color: lavender;
}
.intro a.takeQuiz:active {
  transform: scaleX(0.8);
}
 .quiz-body {
  background-color: rgb(253, 253, 253);
  display: flex;
  justify-content: center;
  align-items: center;
  width: 50%;
  margin: 50px auto;
  flex-direction: column;
  box-shadow: 2px 2px 20px 1px;
  border-radius: 2rem;
}

 .questions {
  margin: 50px;
}


</style>
