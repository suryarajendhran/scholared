<template>
  <div :class="{ modal: true, 'is-active': isActive & !toggleClose }">
    <div class="modal-background"></div>
    <div class="modal-content">
      <div class="box">
        <button class="button is-white" @click="printResults">
          View results array
        </button>
      </div>
    </div>
    <button
      @click="toggleClose = true"
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
      avgResponseTime: null,
      avgChoiceChanges: null,
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
  },
  watch: {
    results: function (val) {
      if (val) {
        const timeData = this.results.answerData.map(
          ({ timeTaken }) => timeTaken
        )
        this.avgResponseTime =
          timeData.reduce((a, b) => a + b, 0) / timeData.length
        console.log(this.avgResponseTime)
      }
    },
  },
  computed: {
    isModalActive: function () {
      if (isActive) {
        return true
      } else {
        return false
      }
    },
  },
}
</script>

<style></style>
