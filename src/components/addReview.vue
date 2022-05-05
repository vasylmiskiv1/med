<template>
  <div v-if="isModalAlert" class="modal__alert">
    <h4 class="modal__alert-title">
       Заповніть обов'язкові поля позначені <span class="asterisk">*</span>
    </h4>
    <button @click="this.isModalAlert = !isModalAlert" type="button" class="modal__alert-button">
      Ок
    </button>
  </div>
  <div v-if="isOpenForm" class="review">
    
    <div class="review__title">
      Залишити оцінку та відгук
    </div>

    <form class="form review__form" @submit="(e) => onSubmit(e)">

<!-- intro -->
      <div class="form__intro">
        <label for="intro">Представтесь: <span class="asterisk">*</span></label>
        <input 
          class ="intro__input" 
          type="text" 
          name="intro" 
          id="intro"
          placeholder="Вкажіть ваше призвіще та ім'я"
          v-model="userData.name"
          :disabled="userData.isUserAnon"
        >
       <input
          class="intro__checkbox"
          type="checkbox"
          id="checkbox" 
          v-model="userData.isUserAnon"
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
      <div class="form__description description">
        <div class="description__title">
          <label for="description">Напишіть відгук:</label>
        </div>
        <div>
          <textarea 
            class="description__textarea"
            id="description" 
            name="description"
            maxlength="500"
            rows="5"
            v-model="this.userData.description"
            placeholder="Вкажіть відгук до 500 символів..."
          ></textarea> 
        </div>
      </div>

      <div class="form__buttons">
        <button class="button button__clear" type="button" @click="handleClearInputs" :disabled="!isFormChanged">
          Очистити
        </button>
         <button type="submit" class="button button__submit">
           Надіслати
        </button>
      </div>
    </form>
  </div>
  <div v-if="successStatus">
    <ModalSuccess />
  </div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';
// modal success
import ModalSuccess from '../modals/ModalSuccess.vue'
export default {
  name: 'addReview',
  props: {
    successStatus: Boolean
  },
  components: {
    ModalSuccess
  },
  data() {
    return {
      // define review
      userData: {
        name: '',
        rating: 0,
        description: '',
        isUserAnon: false,
      },
      isModalAlert: false,
      isOpenForm: true,
      isOpenSuccessModal: false,
    }
  },
  created () {
    console.log(this.successStatus)
  },
  computed: {
    // setup for clear button
    isFormChanged () {
      return (
        this.userData.name ||
        this.userData.rating ||
        this.userData.description ||
        this.userData.isUserAnon
      )
    }
  },
  methods: {
    // clear the input when turns on anon checkbox
    isCheckboxTrue () {
        this.userData.name = ''
    },

    // change stars value
     onChangeRating () {
      const stars = document.getElementById('rate__stars')
      const starsValue = window.getComputedStyle(stars).getPropertyValue('--value')
      this.userData.rating = Number(starsValue)
    },

    // clear inputs
    handleClearInputs () {
      // reset stars amount
      document.getElementById('rate__stars').setAttribute('style', '--value: 0')
      // reset user's inputs(?)
      this.userData = {
        name: '',
        rating: 0,
        description: '',
        isUserAnon: false,
      }
    },

    // form sumbit
    onSubmit (e) {
      e.preventDefault()

      // check form is ready or not
      const isFormReady = () => {
        // set anonymous to user's name input if checkbox is on
        if(this.userData.isUserAnon) {
          this.userData.name = 'Anonymous'
        }
        return this.userData.name && this.userData.rating ? true : false
      }
      
      // if ok send data
      if(isFormReady()) {
        // define current date
        var today = new Date();
        var date = today.getFullYear()+'/'+(today.getMonth()+1)+'/'+today.getDate();
        var time = today.getHours() + ":" + today.getMinutes();
        // set date and id to new review
        const newReview = {
          id: uuidv4(),
          ...this.userData,
          date: `${date} ${time}`,
        }
        // send data
        this.$emit('add-review', newReview)

         // reset stars amount
        document.getElementById('rate__stars').setAttribute('style', '--value: 0')

        // reset review inputs(optional)
        this.userData.name = '',
        this.userData.rating = 0,
        this.userData.description = '',
        this.userData.isUserAnon = false

        // close the form (optional)
          this.isOpenForm = false
          this.isOpenSuccessModal = true

        //temporary hide the form and show successful modal window(rewrite)
        setTimeout(() => {
          this.isOpenForm = true
          setTimeout(() => {
            this.isOpenSuccessModal = false
          },1000)
        }, 3000)
        
      } else {
        // open modal alert
        this.isModalAlert = true
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

/* modal window */
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
    color: #fff;
  }

  /* review form */
  .review {
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
  .review__title {
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

/* review rating */
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
 
  /* veview description */
  .form__description {
    margin-top: 30px;
    display: flex;
    gap: 63px;
  }

  .description__title {
    font-size: 18px;
    flex-grow: 0;
  }

  .description__textarea {
    background: transparent;
    resize: none;
    width: 570px;
    padding: 10px 15px;
    border: 2px solid #000;
    font-size: 18px;
    outline: none;
  }

  .description__textarea::placeholder {
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


  .button__clear:disabled {
    background-color: #e4e1e1;
  }
  
  .button__submit {
    background-color: #B0E3E6;
  }

  .button__submit:hover {
    background-color: #87d9dd;
  }

</style>