<template>
  <h1>This is content inside the index.vue file!</h1>
</template>

<script>
export default {
  head: {
    title: 'Home page 🚀',
    noscript: [{ innerHTML: 'Body No Scripts', body: true }],
    script: [
      { src: '/head.js' },
      // Supported since 1.0
      { src: '/body.js', body: true },
      { src: '/defer.js', defer: '' },
    ],
  },
}
</script>

<style>
</style>
