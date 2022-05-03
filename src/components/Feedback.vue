<template>
  <div v-if="isModalAlert" class="modal__alert">
    <h4 class="modal__alert-title">
       Заповніть обов'язкові поля позначені <span class="asterisk">*</span>
    </h4>
    <button @click="this.isModalAlert = !isModalAlert" type="button" class="modal__alert-button">
      Ок
    </button>
  </div>
  <div class="feebdack">
    
    <div class="feedback__title">
      Залишити оцінку та відгук
    </div>

    <form class="form feedback__form" @submit.prevent="onSubmit">

<!-- intro -->
      <div class="form__intro">
        <label for="intro">Представтесь: <span class="asterisk">*</span></label>
        <input 
          class ="intro__input" 
          type="text" 
          name="intro" 
          id="intro"
          placeholder="Вкажіть ваше призвіще та ім'я"
          @change="onChange($event)"
          v-model="userData.name"
          :disabled="userData.isAnon"
        >
       <input
          class="intro__checkbox"
          type="checkbox"
          id="checkbox" 
          v-model="userData.isAnon"
          @change="isCheckboxTrue"
        >
      </div>

<!-- rating -->
      <div class="form__rate rate">
         <label class="rate__title" for="rate__stars">Оцініть роботу лікаря: <span class="asterisk">*</span></label>
          <input
            class="rate__stars"
            max="5"
            id="rate__stars"
            oninput="this.style.setProperty('--value', this.value)"
            step="1"
            type="range"
            @change="onChangeRating()"
          >
      </div>
      <div class="stars__description">
        <div>
          Дуже погано
        </div>
        <div>
          Погано
        </div>
        <div>
          Нормально
        </div>
        <div>
          Добре
        </div>
        <div>
          Дуже добре
        </div>
      </div>

<!-- review -->
      <div class="form__review">
        <div class="review__title">
          <label for="review">Напишіть відгук:</label>
        </div>
        <div>
          <textarea 
            class="review__textarea"
            id="review" 
            name="review"
            maxlength="500"
            rows="5"
            v-model="this.userData.feedback"
            placeholder="Вкажіть відгук до 500 символів..."
            @change="(e) => onChangeFeedback(e)"
          ></textarea> 
        </div>
      </div>

      <div class="form__buttons">
        <button class="button button__clear" type="button" @click="handleClearInputs">
          Очистити
        </button>
         <button type="submit" class="button button__submit">
           Надіслати
        </button>
      </div>
    </form>
  </div>
</template>

<script>
import StarRating from 'vue-star-rating'
export default {
  name: 'Feedback',
  components: {
      StarRating
  },
  data() {
    return {
      initialUserData: {
      name: '',
      isAnon: false,
      rating: 0,
      feedback: ''
      },
      userData: {...this.initialUserData},
      isModalAlert: false,
      isClearDisabled: false,
    }
  },
  methods: {
    // clear the input when turns on anon checkbox
    isCheckboxTrue () {
      if(this.userData.isAnon) {
        this.userData.name = 'Anonymous'
      }
    },

    onChangeName (e) {
      this.userData.name = e.target.value
    },

     onChangeRating () {
      const stars = document.getElementById('rate__stars')
      const starsValue = window.getComputedStyle(stars).getPropertyValue('--value')
      this.userData.rating = starsValue
    },

    onChangeFeedback (e) {
      this.userData.feedback = e.target.value
    },

    handleClearInputs () {
      // reset stars amount
      document.getElementById('rate__stars').setAttribute('style', '--value: 0')
      // clear all user's values
      this.userData = {...this.initialUserData}
      console.log(this.userData)
    },

    // form sumbit
    onSubmit () {
      // important inputs check
      if(this.userData.isAnon) {
        if(this.userData.rating > 0) {
          console.log(this.userData)
        } else {
          this.isModalAlert = true
        }
      } else {
        if(this.userData.name.length && this.userData.rating > 0) {
          console.log(this.userData)
        } else {
          this.isModalAlert = true
        }
      }
    }
  }
}
</script>

<style>

/* { * } */
  .asterisk {
    color: red;
  }

  .modal__alert {
    background-color: rgba(255, 255, 255, 0.801);
    border: 2px solid #3e758f;
    position: absolute;
    top: 50%;
    left: 50%;
    z-index: 1;
    width: 500px;
    height: 300px;
    display: flex;
    align-content: center;
    flex-direction: column;
    gap: 80px;
    transform: translate(-50%, -50%);
  }

  .modal__alert-title {
    text-align: center;
    margin-top: 50px;
  }
  
  .modal__alert-button {
    padding: 16px 45px;
    border: none;
    border-radius: 5px;
    box-shadow: none;
    margin: 0 auto;
    background-color: #35aac7;
    font-size: 20px;
    transition: 0.2s ease-in;
  }

  .modal__alert-button:hover {
    background-color: #1c90ad;
  }

  .feebdack {
    padding: 20px 18px 20px 50px;
    background: #c0c0c0;
    position: absolute;
    left: 710px;
    top: 280px;
    font-weight: bold;
    color: #3e1daa;
    background: linear-gradient(#F9FBFF, #DAE8FC);
    border: 2px solid #3e758f;
  }

/* article */
  .feedback__title {
    text-align: center;
    font-size: 25px;
    text-transform: uppercase;
  }

/* intro section */
  .form__intro {
    margin-top: 35px;
    display: flex;
    justify-content: start;
    gap: 70px;
    font-size: 18px;
  }

   .intro__input {
    flex-grow: 2;
    background: transparent;
    padding: 8px 0;
    text-align: center;
    font-size: 20px;
    outline: none;
  }

  .intro__input::placeholder {
    font-style: italic;
  }

  .intro__checkbox {
    margin-top: 16px;
    transform: scale(3);
    margin-right: 30px;
  }

  .intro__checkbox::after {
    content: "Анонімно";
    font-size: 6px;
    position: absolute;
    top: 15px;
    margin-left: -6px;
    color: #3e1daa;
    font-weight: bold;
  }

/* feedback rating */
   .form__rate {
    display: flex;
    justify-content: start;
    margin-top: 35px;
    gap: 20px;
  }

  .rate__title {
    font-size: 18px;
  }

  .rate__stars {
    --dir: right;
    --fill: #FFD055;
    --fillbg: rgba(100, 100, 100, 0.15);
    --star: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M12 17.25l-6.188 3.75 1.641-7.031-5.438-4.734 7.172-0.609 2.813-6.609 2.813 6.609 7.172 0.609-5.438 4.734 1.641 7.031z"/></svg>');
    --stars: 5;
    --starsize: 4.5rem;
    --symbol: var(--star);
    --value: 0;
    /* weight and height of star */
    --w: calc(var(--stars) * var(--starsize));
    --x: calc(100% * (var(--value) / var(--stars)));
    /* block of star size */
    block-size: var(--starsize);
    inline-size: var(--w);
    -webkit-appearance: none;
    cursor: pointer;
    background: transparent;
  }

  .rate__stars:hover {
    --value: 5;
  }

  .rate__stars:required {
  border: 1px dashed red;
  }
 
  .rate__stars::-webkit-slider-runnable-track {
    background: linear-gradient(to var(--dir), var(--fill) 0 var(--x), var(--fillbg) 0 var(--x));
    block-size: 100%;
    mask: repeat left center/var(--starsize) var(--symbol);
    -webkit-mask: repeat left center/var(--starsize) var(--symbol);
  }

  .stars__description {
    display: flex;
    gap: 20px;
    font-size: 12px;
    justify-content: center;
    margin-left: 15px;
  }
 
  /* feedback review */
  .form__review {
    margin-top: 30px;
    display: flex;
    gap: 63px;
  }

  .review__title {
    font-size: 18px;
    flex-grow: 0;
  }

  .review__textarea {
    background: transparent;
    resize: none;
    width: 570px;
    padding: 10px 15px;
    border: 2px solid #000;
    font-size: 18px;
    outline: none;
  }

  .review__textarea::placeholder {
    font-style: italic;
  }

  /* form buttons */
  .form__buttons {
    margin-top: 20px;
    display: flex;
    gap: 50px;
    margin-left: 203px;
  }

  .button {
    padding: 14px 53px;
    color: #3e1daa;
    font-weight: bold;
    font-size: 20px;
    transition: 0.2s ease-in;
  }

  .button__clear {
    background-color: #FFF2CC;
  }

  .button__clear:hover {
    background-color: #fadf8e;
  }
  
  .button__submit {
    background-color: #B0E3E6;
  }

  .button__submit:hover {
    background-color: #66dce2;
  }

</style>