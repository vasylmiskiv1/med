<template>
  <div class="reviews__list">
    <div :key="review.id" v-for="review in reviews">
      <Review :review="review"/>
    </div>
    <button class="review__list-button" v-if="reviews.length > 5">
    Завантажити ще
  </button>
  </div>
  
</template>

<script>
import Review  from './Review.vue'
export default {
  name: 'ReviewsList',
  props: {
    reviews: Array,
  },
  components: {
    Review,
  }
}
</script>

<style>
  .reviews__list {
    margin-top: 50px;
  }
  .review__list-button {
    margin-bottom: 100px;
    width: 100%;
    padding: 10px;
    background: #f7f7f7;
    color:#4b4b4b;
    font-size: 20px;
    border-radius: 5px;
    /* because of bootstrap btn styles */
    outline: none !important;
    border: 1px solid transparent;
    transition: 0.3s ease-in;
  }
  .review__list-button:hover {
    color: rgb(39, 39, 39);
  }
</style>