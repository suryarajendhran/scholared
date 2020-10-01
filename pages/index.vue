<template>
  <section
    id="indexSection"
    @click="changeColor"
    :class="{ section: true, white: !darkModeToggle, dark: darkModeToggle }"
  >
    <div class="container columns">
      <nuxt-link :class="linkClass" to="/quiz">
        <p class="title">Quiz Interface</p>
        <p class="subtitle">The ❤️ of Scholared!</p>
      </nuxt-link>
    </div>
  </section>
</template>

<script>
export default {
  head: {
    title: 'Home page 🚀',
    noscript: [{ innerHTML: 'Body No Scripts', body: true }],
  },
  data: function () {
    return {
      linkClass: {
        notification: true,
        'is-warning': true,
        column: true,
        'is-two-fifths': true,
        'has-text-centered': true,
      },
      darkModeToggle: false,
    }
  },
  methods: {
    changeColor: function () {
      this.darkModeToggle = !this.darkModeToggle
    },
  },
}
</script>

<style scoped>
#indexSection {
  height: 100vh;
}

.white {
  background-color: white;
}

.dark {
  background-color: rgb(24, 23, 23);
}
</style>
