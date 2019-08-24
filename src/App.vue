<template>
  <div id="app">
    <b-container class="app-container">
      <b-row class="justify-content-md-center">
        <b-col cols="8" class="app-header">
          <Header
            v-bind:nCorrect="nCorrect"
            v-bind:nQuestion="nQuestion"
            v-bind:resetQuestions="resetQuestions"
          />
          <Question
            v-if="questions.length"
            v-bind:question="questions[questionIdx]"
            v-bind:increaseQuestionIdx="increaseQuestionIdx"
            v-bind:increaseNCorrect="increaseNCorrect"
            v-bind:increaseNQuestion="increaseNQuestion"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Question from './components/Question.vue'

export default {
  name: 'app',
  components: {
    Header,
    Question,
  },
  data: function() {
    return {
      questions: [],
      questionIdx: 0,
      nCorrect: 0,
      nQuestion: 0,
    }
  },
  methods: {
    increaseQuestionIdx: function() {
      this.questionIdx++;
    },
    increaseNCorrect: function() {
      this.nCorrect++;
    },
    increaseNQuestion: function() {
      this.nQuestion++;
    },
    resetQuestions: function() {
      this.questionIdx = 0;
      this.nCorrect = 0;
      this.nQuestion = 0;
    }
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&type=multiple", {
      method: "get"
    })
    .then((response) => {
      return response.json()
    })
    .then((jsonData) => {
      this.questions = jsonData.results
    })
  },
}
</script>

<style>
  .app-container {
    background-color: #fafafa;
    text-align: center;
  }
  .app-header {
    background-color: #cf8;
  }
  .app-question {
    background-color: #cf8;
  }
</style>
