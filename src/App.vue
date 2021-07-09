<template>
  <div>
    <keep-alive>
      <component
          :is="componentInstance"
          :cards-deck="cardsDeck"
          :cards-hand="cardsHand"
          :points="points"
          :price-card="priceCard"
      >
        <template v-slot:user>
          <form @submit.prevent="submit">
            <label for="first">First name</label>
            <input
                type="text" id="first"
                v-model.trim="firstName"
            >
            <div class="error" v-if="!$v.firstName.required">First name is required</div>
            <div class="error" v-if="!$v.firstName.minLength">
              First name must have at least {{ $v.firstName.$params.minLength.min }} letters.
            </div>
            <label for="last">Last name</label>
            <input type="text" id="last" v-model.trim="lastName">
            <div class="error" v-if="!$v.lastName.required">Last name is required</div>
            <div class="error" v-if="!$v.lastName.minLength">
              Last name must have at least {{ $v.lastName.$params.minLength.min }} letters.
            </div>
            <vue-tel-input class="user-page__phone" v-model="phone" ></vue-tel-input>
            <div class="error" v-if="!$v.phone.required">Phone is required</div>
            <button type="submit">Ok</button>
            <p style="color:red;"  v-if="submitStatus === 'ERROR'">Please fill the form correctly.</p>
          </form>
        </template>
        <template v-slot:start>
          <label class="start-page__cards-hand start-page__num-cards">Кол-во карт на руках
            <input type="number" placeholder="Кол-во карт на руках" v-model="countHand" @keyup.enter="startGame">
          </label>
          <label class="start-page__cards-deck start-page__num-cards">Кол-во карт в колоде
            <input type="number" placeholder="Кол-во карт в колоде" v-model="countDeck" @keyup.enter="startGame">
          </label>
          <button @click="startGame" @keyup.enter="startGame">Играть</button>
        </template>
        <template v-slot:img>
          <img :src="deckImg" alt="Deck" @click="takeCard(pickedRadio);  gameWin(); gameOver();">
        </template>
        <template v-slot:radioButton>
          <input type="radio" id="red" value="red" v-model="pickedRadio">
          <label for="red"><span>Red</span></label>
          <br>
          <input type="radio" id="green" value="green" v-model="pickedRadio">
          <label for="green"><span>Green</span></label>
          <br>
          <input type="radio" id="blue" value="blue" v-model="pickedRadio">
          <label for="blue"><span>Blue</span></label>
        </template>
        <template v-slot:end>
          <h1 :style="{color: resGame==='Game Win' ? `darkgreen` : `darkred`}" v-text="resGame"/>
          <p v-text="msg"/>
          <button @click="isEndPage=false; isStartPage=true">Меню</button>
        </template>
      </component>
    </keep-alive>
  </div>
</template>

<script>

import deckImg from "../public/img/Deck.png"
import MaskedInput from 'vue-text-mask'
import {VueTelInput} from 'vue-tel-input';
import 'vue-tel-input/dist/vue-tel-input.css';
import {required, minLength} from 'vuelidate/lib/validators'

class Card {
  _max = 255;
  _min = 0;
  _colors;
  _bigColor = 0;

  constructor() {
    this._colors = {
      red: Math.floor(Math.random() * (this._max - this._min + 1)) + this._min,
      green: Math.floor(Math.random() * (this._max - this._min + 1)) + this._min,
      blue: Math.floor(Math.random() * (this._max - this._min + 1)) + this._min,
      alpha: Math.random().toFixed(2)
    }
    this._bigColor = this._setBigColor();
  }

  getColor() {
    return this._colors
  }

  getBigColor() {
    return this._bigColor
  }

  getRGBA() {
    return `rgba(${this._colors.red},${this._colors.green},${this._colors.blue},${this._colors.alpha})`
  }

  _setBigColor() {
    const value = Object.values(this.getColor());
    const index = value.indexOf(Math.max.apply(null, value));
    return Object.keys(this.getColor())[index];
  }
}

export default {
  name: 'App',
  data() {
    return {
      isStartPage: false,
      isEndPage: false,
      isGamePage: false,
      countHand: 3,
      countDeck: 4,
      cardsHand: [],
      cardsDeck: [],
      points: {
        red: 0,
        green: 0,
        blue: 0,
      },
      resGame: '',
      msg: '',
      pickedRadio: "red",
      priceCard: 100,
      phone: '',
      firstName: '',
      lastName:'',
      deckImg,
      submitStatus: null
    }
  },
  validations: {
    firstName: {
      required,
      minLength: minLength(3)
    },
    lastName:{
      required,
      minLength: minLength(2)
    },
    phone:{
      required,
    }
  },
  components: {
    'start-page': () => import('@/components/StartPage'),
    'game-page': () => import('@/components/GamePage'),
    'end-page': () => import('@/components/EndPage'),
    'user-page': () => import('@/components/UserPage'),
    'vue-tel-input': VueTelInput,
    MaskedInput
  },
  computed: {
    componentInstance() {
      let name = "start-page";
      if (this.isStartPage === true) {
        name = "start-page"
      } else if (this.isGamePage === true) {
        name = "game-page"
      } else if (this.isEndPage === true) {
        name = "end-page"
      } else {
        name = "user-page"
      }
      return name;
    }
  },
  methods: {
    submit() {

      this.$v.$touch()
      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR'
        this.isStartPage=false;
      } else {
        this.submitStatus = 'OK';
        this.isStartPage=true;
      }
    },
    startGame() {
      this.resetVal();
      this.initCardsHand(this.countHand);
      this.initCardsDeck(this.countDeck);
      this.setPointsStart();
    },
    gameOver() {
      this.resGame = "Game Over";
      if (this.cardsDeck.length === 0) {
        this.msg = "Закончились карты в колоде";
        this.isEndPage = true;
        this.isGamePage = false;
      } else if (Object.values(this.points).every(el => el === 0)) {
        this.msg = "Очки равны нулю ";
        this.isEndPage = true;
        this.isGamePage = false;
      }
    },
    gameWin() {
      if (Object.values(this.points).every(el => el === this.points[0])) {
        this.resGame = "Game Win";
        this.msg = "Вы выиграли!!"
        this.isEndPage = true;
        this.isGamePage = false;
      }
    },
    initCardsHand(countHand) {
      for (let i = 0; i < countHand; i++)
        this.cardsHand.push(new Card());
    },
    initCardsDeck(countDeck) {
      for (let i = 0; i < countDeck; i++)
        this.cardsDeck.push(new Card());
    },
    takeCard(radio) {
      if (this.cardsDeck.length)
        if (this.points[radio] >= this.priceCard) {
          this.points[radio] -= this.priceCard;
          this.cardsHand.push(this.cardsDeck.shift());
          this.setPoints();
        } else {
          const h1 = document.querySelectorAll(".game-page__points h1")
          console.log(h1);
          const index = Object.keys(this.points).indexOf(radio);
          h1[index].classList.add("shake")
          setTimeout(() => h1[index].classList.remove('shake'), 500);
        }
    },
    resetVal() {
      this.isStartPage = false;
      this.isGamePage = true;
      this.cardsDeck = [];
      this.cardsHand = [];
      Object.keys(this.points).forEach(el => this.points[el] = 0);
    },
    setPointsStart() {
      this.cardsHand.forEach(el => {
        const color = el.getBigColor();
        Object.keys(this.points).forEach(el1 => {
          el1 === color ? this.points[el1] += el.getColor()[color] : false
        })
      });
    },
    setPoints() {
      const el = this.cardsHand[this.cardsHand.length - 1]
      const color = el.getBigColor();
      Object.keys(this.points).forEach(el1 => {
        el1 === color ? this.points[el1] += el.getColor()[color] : false
      });
    }
  }
}
</script>

<style lang="scss">
@import "src/style/main";

.error {
  color: red;
  margin-left:10px;
  align-self: flex-start;
}
</style>
