<template>
  <div id="app">
    <Header />
    <div class="container">
      <DocProfile :reviews="reviews"/>
      <CreateReview @create-review="createReview" />
      <ReviewsList :reviews ="reviews"/>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import DocProfile from './components/DocProfile.vue'
import CreateReview from './components/CreateReview.vue'
import ReviewsList from './components/ReviewsList.vue'
export default {
  name: 'app',
  components: {
    Header,
    DocProfile,
    CreateReview,
    ReviewsList,
  },
  data () {
    return {
      reviews: []
    }
  },
  methods: {
    createReview (review) {
      this.reviews = [review, ...this.reviews]
    }
  },
  // fake server fetch (react did mount)
  created() {
    this.reviews = [
      {
        id: 1,
        name: 'Віктор',
        rating: 4,
        description: 'Typesetting industry. Lorem Ipsum has been the industrys',
        isAnon: false,
      },
      {
        id: 2,
        name: 'Валерія',
        rating: 2,
        description: 'Typesetting industry.',
        isAnon: false,
      },
      {
        id: 3,
        name: 'Anonymous',
        rating: 1,
        description: 'Lorem Ipsum has been the industrys',
        isAnon: true,
      },
      {
        id: 4,
        name: 'Anonymous',
        rating: 1,
        description: 'Lorem Ipsum has been the industrys.Lorem Ipsum has been the industrys.Lorem Ipsum has been the industrys.Lorem Ipsum has been the industrys.Lorem Ipsum has been the industrys.Lorem Ipsum has been the industrys.Lorem Ipsum has been the industrys',
        isAnon: true,
      },
      {
        id: 3,
        name: 'Анна',
        rating: 5,
        description: 'Чудово',
        isAnon: false,
      }
    ]
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');
  #app {
    font-family: Roboto, sans-serif;
  }
</style>