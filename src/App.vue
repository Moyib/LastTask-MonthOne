<template>

  <div class="main-body">
    <div id="app" class="quiz-body">
      <div class="intro">
        <h2>How much of a patriot are you?</h2>
        <a role="button" class="take-quiz" v-if="!showQuiz" @click="showQuiz = true">Take Quiz</a>
        <questions v-if="showQuiz" class="questions"></questions>
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
      showQuiz: false
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

.intro a.take-quiz {
  padding: .5rem;
  border: 1px solid gainsboro;
  border-radius: .5rem;
  cursor: pointer;
  transition: 0.3s;
}
.intro a.take-quiz:hover {
  background-color: lavender;
}
.intro a.take-quiz:active {
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
