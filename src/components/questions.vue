<template>
 <transition 
    :duration="{ enter: 600, leave: 300 }"
    enter-active-class="animated zoomIn"
    leave-active-class="animated zoomOut" mode="out-in"> 
   
    <div class="question-background">
      <div class="question-container">

          <div class="quiz-questions" v-if="questionIndex < quizData.questions.length">
            
          <header>
            <div class="progress-container">
              <div class="progress"
                   :style="{width: (questionIndex / quizData.questions.length) * 100 + '%'}"
                   :value="(questionIndex / quizData.questions.length) * 100"
                   max="100"
              >
              {{ (questionIndex / quizData.questions.length) * 100 }}%
              </div>
              <p>
                {{ (questionIndex / quizData.questions.length) * 100 }}% complete
              </p>
            </div>
          </header>

          <h2 class="question-title">
            {{ quizData.questions[questionIndex].text }}
          </h2>

          <div class="option-content">
            <div
                class="option"
                v-for="(response, index) in quizData.questions[questionIndex].responses"
                @click="selectOption(index)"
                :class="{'is-selected' : userResponses[questionIndex] == index}"
                :key="index"
            >
              {{ response.option }}. {{ response.text }}
            </div>
          </div>

          <footer class="quiz-footer">
            <nav class="footer-buttons" role="navigation">
              <a class="button" @click="prev" :disabled="questionIndex < 1">Go back</a>

              <a class="button"
                :class="(userResponses[questionIndex] == null) ? '' : 'is-active'"
                @click="next"
                :disabled="questionIndex >= quizData.questions.length"
              >
                {{ (userResponses[questionIndex]==null) ? 'Skip' : 'Next' }}
              </a>
            </nav>
          </footer>
        </div> <!-- End of the question content -->

        <!-- Quiz Results -->
        <div class="quiz-end" v-if="quizEnd" :key="questionIndex">
        
          <p class="scorecard">
            Total score is: {{ score() }} / {{ quizData.questions.length }}
          </p>
          <a class="final-button" @click="restart">Take quiz again &nbsp;&nbsp;<font-awesome-icon icon="redo" /></a>
        </div>
      </div>
    </div>
 </transition>
</template>

<script>
export default {
  name: 'questions',
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
  },

  computed: {
    quizEnd() {
      return this.questionIndex >= this.quizData.questions.length
    }
  }
} 
</script>
<style scoped>

.question-background {
    display: flex;
    justify-content: center;
    align-items: center;
}

.animated {
transition-duration: 0.1s;
}

.progress-container {
  transition: width 500ms;
}

.progress-container .progress {
  border-bottom: 1px solid #C2CECE;
  padding-bottom: 5px;
  background-color: green;
  color: white;
  padding-left: 3px;
}

.option {
  margin: 7px;
  padding: 10px;
  background-color: gainsboro; 
  border-radius: 1rem;
  border: transparent 1px solid;
  cursor: pointer;
}

.option:hover {
  background-color: rgb(193, 220, 243);
}

.option:active {
  transform: translate(5px) rotate(2deg);
}
.is-selected {
  background-color: white;
  border-color: lightblue;
}

.quiz-footer {
  margin-top: 30px;
}

.footer-buttons {
  display: flex;
  justify-content: space-between;
}

.footer-buttons .button, .final-button {
  padding: .5rem;
  border: 1px solid gainsboro;
  border-radius: .5rem;
  cursor: pointer;
  transition: 0.3s;
}
.footer-buttons .button:hover, .final-button:hover {
  background-color: lavender;
}
.footer-buttons .button:active, .final-button:active {
  transform: scaleX(0.8);
}
.scorecard {
  margin-bottom: 30px;
}

</style>>
