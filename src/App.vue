<template>
  <div id="app">
    <Header />
    <div class="container">
      <DocProfile />
      <Feedback />
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import DocProfile from './components/DocProfile.vue'
import Feedback from './components/Feedback.vue'
export default {
  name: 'app',
  components: {
    Header,
    DocProfile,
    Feedback,
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');
  #app {
    font-family: Roboto, sans-serif;
  }
</style>