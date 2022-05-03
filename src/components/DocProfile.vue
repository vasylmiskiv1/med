<template>
  <div class="profile">
    <div class="profile__about">
      <div class="profile__name">
        Тестова Оксана Василівна
      </div>
      <div class="profile__rating">
        <star-rating
          :max-rating ="3"
          :star-size = "33"
          :rating = "3"
          :show-rating = "false"
          :read-only = "true"
        />
      </div>
      <div class="rating__info">
        (Середня оцінка: 3)
      </div>
    </div>

    <div class="profile__card">
      <div class="profile__photo">
        <img 
          class="profile__img"
          src="../assets/profile.jpg" 
          alt="profile-photo"

        >
      </div>
      <div class="profile__description">
        <div class = "profile__fields">
          <ul class="list fields__list">
            <li>
              Місто:
            </li>
            <li>
              Місце роботи:
            </li>
            <li>
              Заклад:
            </li>
          </ul>
        </div>
        <div class = "profile__fields-values">
          <ul class="list fields-values__list">
            <li>
              Львів
            </li>
            <li>
              Львів, площа Ринок, 10
            </li>
            <li>
              <a href="#">Відділення надання екстренної домоаоги чергвоим лікарем у закладі стаціонарної допомоги імені Пилипіва Юрія </a>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
 
</template>

<script>
  import StarRating from 'vue-star-rating'
  export default {
    name: 'DocProfile',
     components: {
      StarRating
    }
  }
 
</script>

<style>
  .profile {
    margin-top: 30px;
  }
  .profile__about {
    display: flex;
    align-items: center;
    justify-content: start;
    gap: 17px;
  }
  .profile__name {
    color: #46cad6;
    font-size: 25px;
    margin-left: 10px;
    text-transform: uppercase;
  }

  .rating__info {
    font-size: 13px;
  }

  .profile__card {
    display: flex;
    justify-content: flex-start;
    gap: 20px;
    padding: 10px;
    border: 1px solid #c0c0c0;
  }

  .profile__img {
    object-fit: cover;
    width: 219px;
    height: 243px;
  }

  .profile__description {
    font-size: 15px;
    display: flex;
    gap: 10px;
    width: 650px;
  }

  .list {
    list-style: none;
  }

  .profile__fields {
    font-weight: bold;
    min-width: 150px;
  }
</style>