<template>
  <div class="home">
    <HomePage />
  </div>
</template>

<script>
import HomePage from '@/components/HomePage/HomePage.vue'

export default {
  name: 'HomeView',
  components: {
    HomePage
  }
}
</script>

<style scoped>
  body {
    background-color: #2c3e50;
  }
</style>
