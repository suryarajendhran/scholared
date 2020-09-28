<template>
  <div id="quizBody">
    <ProgressBar />
    <div id="questionCounter">
      Question {{ questionsAttempted }}<span id="totalQuestions">/10</span>
    </div>
    <hr style="border-style: dotted; border-width: 1px" />
    <div id="question">{{ question }}</div>
    <div class="choices">
      <div
        :class="{ choice: true, selected: selected == choice }"
        v-for="choice in choices"
        :key="choice"
        @click="selected = choice"
      >
        <div class="choiceText">{{ choice }}</div>
        <!-- <input
          type="radio"
          name="choices"
          v-model="selected"
          :value="choice"
          class="checkBox"
        /> -->
      </div>
    </div>
    <div class="plainButton bouncy" @click="nextQuestion">Submit!</div>
    <div
      v-show="selected"
      :class="{ status: true, correct: selected == correctAnswer }"
    >
      Selected answer: {{ selected }}
    </div>
  </div>
</template>

<script>
const axios = require('axios')
const shuffle = require('shuffle-array')
export default {
  data() {
    let question = null
    let choices = ['This is opt 1', 'This is opt 2', 'This is opt 3']
    let selected = null
    let correctAnswer = null
    let currentQuestionNumber = 1
    let correctAnswerCount = 0
    let questionsAttempted = 0
    return { choices, selected, correctAnswer, question, currentQuestionNumber, questionsAttempted }
  },
  methods: {
    nextQuestion: function() {
      if (this.selected == this.correctAnswer) {
        this.correctAnswerCount++
      }
      this.questionsAttempted++
      this.$fetch()
    },
  },
  async fetch() {
    await axios
      .get(
        'https://opentdb.com/api.php?amount=1&category=27&difficulty=easy&type=multiple'
      )
      .then((response) => {
        console.log(response)
        console.log(response.data.results[0])
        let result = response.data.results[0]
        this.question = result.question
        this.choices = [...result.incorrect_answers, result.correct_answer]
        shuffle(this.choices)
        this.correctAnswer = result.correct_answer
      })
  },
  // async asyncData({ params }) {
  //   const { data } = await axios.get(`https://my-api/posts/${params.id}`)
  //   return { title: data.title }
  // }
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
  margin: 10% 0% 10% 0%;
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

.checkBox {
  opacity: 0;
  margin: 10% 15%;
  border-radius: 50%;
}

.selected {
  background: #204c6e;
  border: 4px solid #243150;
}

.status {
  color: #939cc5;
  font-size: 2.5rem;
}

.correct {
  color: greenyellow;
}

.bouncy {
  animation: bouncy 5s infinite linear;
  position: relative;
}
@keyframes bouncy {
  0% {
    top: 0em;
  }
  40% {
    top: 0em;
  }
  43% {
    top: -0.9em;
  }
  46% {
    top: 0em;
  }
  48% {
    top: -0.4em;
  }
  50% {
    top: 0em;
  }
  100% {
    top: 0em;
  }
}

.plainButton {
  display: inline-block;
  padding: 0.35em 1.2em;
  border: 0.1em solid #ffffff;
  margin: 0 0.3em 0.3em 0;
  border-radius: 0.5em;
  box-sizing: border-box;
  text-decoration: none;
  cursor: pointer;
  color: #f6f6f6;
  text-align: center;
  transition: all 0.2s;
}
.plainButton:hover {
  color: #000000;
  background-color: #ffffff;
}

@media all and (max-width: 30em) {
   .plainButton {
    display: block;
    margin: 0.4em auto;
  }
}
</style>
