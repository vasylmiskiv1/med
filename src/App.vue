<template>

  <div id="app">
    <Header />
    <div class="container">
      <DocProfile v-if="reviews.length" :reviews="reviews"/>
      <addReview @add-review="addReview" :successStatus="successStatus"/>
      <ReviewsList :reviews="reviews"/>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import DocProfile from './components/DocProfile.vue'
import addReview from './components/addReview.vue'
import ReviewsList from './components/ReviewsList.vue'
export default {
  name: 'app',
  components: {
    Header,
    DocProfile,
    addReview,
    ReviewsList,
  },
  data () {
    return {
      reviews: [],
      successStatus: false,
    }
  },
  methods: {
    async addReview (newReview) {
      try {
        const res = await fetch('http://localhost:5000/reviews', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
           body: JSON.stringify(newReview)
        })

        if(res.ok) {
          this.successStatus = true
          setTimeout(() => {
            this.successStatus = false
          },5000)
        }
        // get our posted new review
        const data = await res.json()
        // put it in our list
        this.reviews = [data, ...this.reviews]
      } catch(err) {
        console.error(err)
      }
    },

    async fetchReviews () {
      const res = await fetch(`http://localhost:5000/reviews`)
      const data = await res.json()
      return data
    }
  },

  // fetching from the server (mounted)
  async created() {
    this.reviews =  await this.fetchReviews()
    // sorted by date fetched reviews
    this.reviews.sort((a,b) => new Date(b.date) - new Date(a.date));
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');
  #app {
    font-family: Roboto, sans-serif;
  }
</style>