<template>
  <div class="reviews__list">
    <div :key="review.id" v-for="review in reviews">
      <Review :review="review"/>
    </div>
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
  },
}
</script>
<style>
  .reviews__list {
    margin-top: 50px;
  }
</style>