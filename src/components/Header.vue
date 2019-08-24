<template>
  <div>
    <b-nav>
      <b-nav-item disabled>{{ scoreText }}</b-nav-item>
      <b-nav-item active v-on:click="resetQuestions">Reset</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
  export default {
    props: {
        nCorrect: Number,
        nQuestion: Number,
        resetQuestions: Function,
    },
    computed: {
      scoreText: function() {
        return `Question ${this.nCorrect} from ${this.nQuestion}`
      }
    },
  }
</script>