<template>
  <div class="review__item">
    <div class="review__name">
      {{review.name}}
    </div>
    <div class="review__date">
      {{review.date}}
    </div>
    <div class="review__rating">
      <star-rating
        :max-rating ="5"
        :star-size = "20"
        :rating = "review.rating"
        :show-rating = "false"
        :read-only = "true"
      />
    </div>
    <div class="review__description">
      {{review.description}}
    </div>
  </div>
</template>

<script>
import StarRating from 'vue-star-rating'
export default {
  name: 'Review',
  props: {
    review: Object,
  },
  components: {
      StarRating
  }
}
</script>

<style>
  .review__item {
    margin-top: 20px;
    padding: 18px;
    border: 1px solid #c0c0c0;
    border-radius: 5px;
    min-height: 100px;
  }
  
  .review__name {
    font-size: 18px;
    font-weight: bold;
  }

  .review__description {
    margin-top: 10px;
  }

</style>