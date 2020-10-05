<template>
  <div :class="{ modal: true, 'is-active': isActive }">
    <div class="modal-background"></div>
    <div class="modal-content">
      <div class="box">
        <h1 class="is-size-1 has-text-centered">Results</h1>
        <p class="my-2 is-size-5">
          Percentage of correct answers:
          <span class="is-pulled-right has-text-primary"
            >{{ correctPercentage }}%</span
          >
        </p>
        <p class="my-2 is-size-5">
          Time taken to respond to questions:
          <span class="is-pulled-right has-text-primary">{{
            formattedAvgResponseTime
          }}</span>
        </p>
        <p class="my-2 is-size-5">
          Number of choice changes:
          <span class="is-pulled-right has-text-primary">{{
            formattedChoiceChanges
          }}</span>
        </p>
        <!-- <button class="button is-white" @click="printResults">
          View results array
        </button> -->
      </div>
    </div>
    <button
      @click="
        {
          $emit('close-modal')
        }
      "
      class="modal-close is-large"
      aria-label="close"
    ></button>
  </div>
</template>

<script>
export default {
  name: 'ResultsModal',
  data() {
    return {
      toggleClose: false,
    }
  },
  props: {
    // {correctAnswers,
    //  answerData
    //    {timeTaken, question, correctAnswer,
    //      selectedAnswer, isCorrect, choiceChanges},
    //  totalQuestions}
    results: Object,
    isActive: false,
  },
  methods: {
    printResults: function () {
      console.log(this.results)
    },
    formatFloat: function (floatValue) {
      return `${Math.round(floatValue * 100) / 100}`
    },
  },
  computed: {
    correctPercentage: function () {
      if (this.results) {
        return (this.results.correctAnswers / this.results.totalQuestions) * 100
      }
      return 0
    },
    formattedAvgResponseTime: function () {
      if (!this.results) {
        return 0
      }

      const timeData = this.results.answerData.map(({ timeTaken }) => timeTaken)

      const avgResponseTime =
        timeData.reduce((a, b) => a + b, 0) / timeData.length

      return `${this.formatFloat(avgResponseTime)} secs.`
    },
    formattedChoiceChanges: function () {
      if (!this.results) {
        return 0
      }

      const choiceData = this.results.answerData.map(
        ({ choiceChanges }) => choiceChanges
      )

      const avgChoices =
        choiceData.reduce((a, b) => a + b, 0) / choiceData.length

      return `${this.formatFloat(avgChoices)}`
    },
  },
}
</script>

<style></style>
