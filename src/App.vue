<template>
  <StartPage v-if="isStartPage">
    <label class="start-page__cards-hand start-page__num-cards">Кол-во карт на руках
      <input type="number" placeholder="Кол-во карт на руках" v-model="countHand" @keyup.enter="startGame">
    </label>
    <label class="start-page__cards-deck start-page__num-cards">Кол-во карт в колоде
      <input type="number" placeholder="Кол-во карт в колоде" v-model="countDeck" @keyup.enter="startGame">
    </label>
    <button @click="startGame" @keyup.enter="startGame">Играть</button>
  </StartPage>
  <GamePage
      v-if="isGamePage"
      v-bind:cards-deck="cardsDeck"
      v-bind:cards-hand="cardsHand"
      v-bind:points="points"
      v-bind:price-card="priceCard"
  >
    <template v-slot:img>
      <img src="@/img/Deck.png" alt="Deck" @click="takeCard(pickedRadio);  gameWin(); gameOver();">
    </template>
    <template v-slot:radioButton>
      <input type="radio" id="red" value="red" v-model="pickedRadio">
      <label for="red"><span>Red</span></label>
      <br>
      <input type="radio" id="green" value="green" v-model="pickedRadio">
      <label for="green"><span>Green</span></label>
      <br>
      <input type="radio" id="blue" value="blue" v-model="pickedRadio">
      <label for="blue">Blue</label>
    </template>
  </GamePage>
  <EndPage v-if="isEndPage">
    <h1 :style="{color: resGame==='Game Win' ? `darkgreen` : `darkred`}" v-text="resGame"/>
    <p v-text="msg"/>
    <button @click="isEndPage=false; isStartPage=true">Меню</button>
  </EndPage>
</template>

<script>
import StartPage from '@/components/StartPage';
import GamePage from "@/components/GamePage";
import EndPage from "@/components/EndPage";

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
      isStartPage: true,
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
      priceCard: 100
    }
  },
  components: {
    StartPage,
    GamePage,
    EndPage
  },
  methods: {
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
      if (this.points.red === this.points.green && this.points.red === this.points.blue) {
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

#app {
  .start-page {
    &__num-cards {
      display: flex;
      flex-direction: column;
      justify-self: center;
      font-size: 20px;

      & input {
        height: 40px;
        margin-top: 5px;
        border: 1px #808080 solid;
        border-radius: 5px;
      }
    }

    & button {
      grid-column: 1/3;
      grid-row: 3/3;
      width: 150px;
      height: 50px;
      justify-self: center;
      color: #00bb00;
      border: 1px #00bb00 solid;
      border-radius: 5px;
      font-size: 25px;
    }

    & button:hover {
      background-color: #00bb00;
      color: #fff;
    }
  }
  .game-page {

    &__cards-deck {
      & img {
        width: 250px;
        height: 270px;
      }

      & img:hover {
        opacity: 0.7;
      }
    }
    &__buy-card {
      $primary: #6743ee;
      $seconday: #9F9F9F;

      & input {
        //margin: 10px;
        vertical-align: middle;
        width: 20px;
        height: 20px;
        border-radius: 10px;
        background: none;
        border: 0;
        //box-shadow: inset 0 0 0 1px $seconday;
        box-shadow: inset 0 0 0 1.5px $seconday;
        appearance: none;
        padding: 0;
        margin: 0;
        transition: box-shadow 150ms cubic-bezier(.95, .15, .5, 1.25);
        pointer-events: none;

        &:focus {
          outline: none;
        }

        &:checked {
          box-shadow: inset 0 0 0 6px $primary;
        }
      }

      & label {
        padding: 6px;
        border-radius: 50px;
        display: inline-flex;
        cursor: pointer;
        transition: background .2s ease;
        margin: 8px 0;
        -webkit-tap-highlight-color: transparent;

        &:hover, &:focus-within {
          background: rgba($seconday, .1);
        }

        & span {
          vertical-align: middle;
          display: inline-block;
          line-height: 20px;
          padding: 0 8px;
        }
      }
    }
  }
  .end-page {
    & h1 {
      font-size: 60px;
      margin: 0;
      padding: 0;
    }

    & p {
      font-size: 20px;
    }

    & button {
      grid-column: 1/3;
      grid-row: 3/3;
      width: 150px;
      height: 50px;
      justify-self: center;
      color: #557fc7;
      border: 1px #557fc7 solid;
      border-radius: 5px;
      font-size: 25px;
    }

    & button:hover {
      background-color: #557fc7;
      color: #fff;
    }

  }
}
</style>
