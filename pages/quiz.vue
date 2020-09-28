<template>
  <div id="quizBody" class="fade-in">
    <ProgressBar />
    <div id="questionCounter">
      Question {{ questionsAttempted }}<span id="totalQuestions">/10</span>
    </div>
    <hr style="border-style: dotted; border-width: 1px" />
    <div id="question" v-html="question"></div>
    <div class="choices">
      <div
        :class="{ choice: true, selected: selected == choice }"
        v-for="choice in choices"
        :key="choice"
        @click="selected = choice"
      >
        <div class="choiceText" v-html="choice"></div>
      </div>
    </div>
    <button
      :class="{
        button: true,
        'is-outline': true,
        'is-loading': isLoading,
        'is-fullwidth': true,
        'is-large': true,
      }"
      @click="nextQuestion"
    >
      Submit!
    </button>
    <div
      v-show="selected"
      :class="{ status: true, correct: selected == correctAnswer }"
    >
      Selected answer: {{ selectedText }}
    </div>
  </div>
</template>

<script>
const axios = require('axios')
const shuffle = require('shuffle-array')
const htmlToText = require('html-to-text')
export default {
  head: {
    title: 'Quiz | Scholared',
    meta: [{ charset: 'utf-8' }],
  },
  data() {
    return {
      choices: null,
      selected: null,
      correctAnswer: null,
      question: null,
      currentQuestionNumber: 1,
      questionsAttempted: 0,
      isLoading: false,
    }
  },
  methods: {
    async nextQuestion() {
      if (this.selected == this.correctAnswer) {
        this.correctAnswerCount++
      }
      await this.$fetch()
    },
  },
  computed: {
    selectedText: function () {
      return htmlToText.fromString(this.selected)
    },
  },
  async fetch() {
    this.isLoading = true
    await axios
      .get(
        'https://opentdb.com/api.php?amount=1&category=9&difficulty=easy&type=multiple'
      )
      .then((response) => {
        let result = response.data.results[0]
        this.question = result.question
        this.choices = [...result.incorrect_answers, result.correct_answer]
        shuffle(this.choices)
        this.correctAnswer = result.correct_answer
        this.questionsAttempted++
        this.selected = null
        this.isLoading = false
      })
  },
  async asyncData({ params }) {
    await axios
      .get(
        'https://opentdb.com/api.php?amount=1&category=9&difficulty=easy&type=multiple'
      )
      .then((response) => {
        console.log(response)
        console.log(response.data.results[0])
        let result = response.data.results[0]
        let question = result.question
        let choices = [...result.incorrect_answers, result.correct_answer]
        shuffle(choices)
        let correctAnswer = result.correct_answer
        return { result, question, choices, correctAnswer }
      })
  },
  loading: true,
}
</script>

<style>
body {
  background-color: #252c4a;
}
#quizBody {
  /* height: 100%; */
  /* width: 100%; */
  padding: 5% 10%;
  background-color: #252c4a;
  /* background-color: aliceblue; */
}

#totalQuestions {
  font-size: 1.5rem;
  color: #6d759a;
}

#questionCounter {
  color: #939cc5;
  font-size: 2.5rem;
}

#question {
  color: #ffffff;
  font-size: 4rem;
}

.choices {
  margin: 10% 0% 5% 0%;
  display: grid;
  grid-template-columns: 50% 50%;
  column-gap: 10px;
  row-gap: 10px;
}

.choice {
  margin: 0px 0px 1% 0px;
  padding: 0 0 0 1%;
  border: 4px solid #204c6e;
  border-radius: 10px;
  color: #ffffff;
  font-size: 2rem;
  background-color: #243150;
  display: grid;
  grid-template-columns: 90% 10%;
  cursor: pointer;
}

.choice:hover {
  border: 4px solid #3875a5;
}

.checkBox {
  opacity: 0;
  margin: 10% 15%;
  border-radius: 50%;
}

.selected {
  background: #204c6e;
  border: 4px solid #3875a5;
}

.status {
  color: #939cc5;
  font-size: 2.5rem;
}

.correct {
  color: greenyellow;
}

.fade-in {
  -webkit-animation: fade-in 1.2s cubic-bezier(0.39, 0.575, 0.565, 1) both;
  animation: fade-in 1.2s cubic-bezier(0.39, 0.575, 0.565, 1) both;
}

@-webkit-keyframes fade-in {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
@keyframes fade-in {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@media all and (max-width: 30em) {
   .plainButton {
    display: block;
    margin: 0.4em auto;
  }
}
</style>
