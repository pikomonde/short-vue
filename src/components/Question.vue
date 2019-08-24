<template>
  <b-jumbotron>
    <template slot="lead">
      <div v-html="question.question"></div>
    </template>

    <hr class="my-4">

    <p>
      <b-list-group>
        <b-list-group-item
          v-for="(option, optionIdx) in options"
          v-bind:key="optionIdx"
          v-on:click="selectOption(optionIdx)"
          v-bind:class="listOptionClass(optionIdx)"
        >
          <div v-html="option"></div>
        </b-list-group-item>
      </b-list-group>
    </p>

    <b-button
      variant="primary"
      v-on:click="answerQuestion"
      v-if="!isAnswered"
      v-bind:disabled="selectedIdx === null"
    >
      Answer
    </b-button>
    <b-button
      variant="secondary"
      v-on:click="nextQuestion"
      v-if="isAnswered"
    >
      Next
    </b-button>
  </b-jumbotron>
</template>

<script>
  import _ from 'lodash'

  export default {
    props: {
      question: Object,
      increaseQuestionIdx: Function,
      increaseNCorrect: Function,
      increaseNQuestion: Function,
    },
    data: function() {
      return {
        isAnswered: false,
        selectedIdx: null,
        options: [],
        correctAnswer: null,
      }
    },
    methods: {
      selectOption: function(idx) {
        if (!this.isAnswered) {
          this.selectedIdx = idx;
        }
      },
      initQuestion: function () {
          // init asnwered logic
          this.isAnswered = false 
          this.selectedIdx = null

          // init options and answer
          this.options = [...this.question.incorrect_answers];
          this.options.push(this.question.correct_answer);
          this.options = _.shuffle(this.options)
          this.correctAnswer = this.options.indexOf(this.question.correct_answer)
      },
      answerQuestion: function() {
        this.isAnswered = true

        // increase score
        if (this.selectedIdx === this.correctAnswer) {
          this.increaseNCorrect();
        }
        this.increaseNQuestion();
      },
      nextQuestion: function() {
        this.increaseQuestionIdx();
      },
      listOptionClass: function(idx) {
        if (!this.isAnswered && (idx === this.selectedIdx)) {
          return 'list-group-item-selected'
        } else if (this.isAnswered && (idx === this.selectedIdx)) {
          if (this.selectedIdx === this.correctAnswer) {
            return 'list-group-item-answered-right'
          } else {
            return 'list-group-item-answered-wrong'
          }
        } else if (this.isAnswered && (idx === this.correctAnswer)) {
          return 'list-group-item-correct-answer'
        } else {
          return 'list-group-item'
        }
      },
    },
    watch: {
      question: {
        immediate: true,
        handler() { 
          this.initQuestion()
        }
      } 
    }
  }
</script>

<style scoped>
  .list-group-item:hover {
    background-color: #fff;
  }
  
  .list-group-item:hover {
    background-color: #cde;
    cursor: pointer;
  }

  .list-group-item-selected {
    background-color: #ccc;
  }

  .list-group-item-selected:hover {
    background-color: #9ab;
  }

  .list-group-item-answered-right {
    background-color: #4f4;
  }

  .list-group-item-answered-wrong {
    background-color: #f44;
  }

  .list-group-item-correct-answer {
    background-color: #44f;
  }


</style>