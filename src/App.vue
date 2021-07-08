<template>
<<<<<<< HEAD
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
=======
  <div class="startPage_Wrap" v-if="isStartPage">
    <div class="startPage">
      <h1>GameDich</h1>
      <label class="startPage__countHand startPage__count">Кол-во карт на руках
        <input type="number" placeholder="Кол-во карт на руках" v-model="countHand" @keyup.enter="startGame">
      </label>
      <label class="startPage__countDeck startPage__count">Кол-во карт в колоде
        <input type="number" placeholder="Кол-во карт в колоде" v-model="countDeck" @keyup.enter="startGame">
      </label>
      <button @click="startGame" @keyup.enter="startGame">Играть</button>
    </div>
  </div>
  <div class="gamePage_Wrap" v-if="isGamePage">
    <div class="gamePage">
      <div class="gamePage__cardsDeck">
        <img src="@/img/Deck.png" alt="Deck" @click="takeCard(pickedRadio);  gameWin(); gameOver();">
        <h1>Карт в колоде: {{ cardsDeck.length }}</h1>
      </div>
      <div class="gamePage__cardsHand">
        <div class="gamePage__cardsHand__card__wrap"
             v-for="(card, index ) in cardsHand"
             :key="index"
             :style="{backgroundColor: `rgba(${card.getColor().Red},${card.getColor().Green},${card.getColor().Blue},${card.getColor().Alpha})`}"

        >
          <div
              class="gamePage__cardsHand__card"

              :style="{backgroundColor: `rgba(${card.getColor().Red},${card.getColor().Green},${card.getColor().Blue},${card.getColor().Alpha})`}"
          >
            <p v-for="(color,name) in card.getColor()" :key="name"
               :style="{color : name===card.getBigColor() ? `red`: `black`}"
            >
              {{ name }}: {{ color }}
            </p>
          </div>
        </div>
      </div>
      <div class="gamePage__points">
        <h1
            v-for="(point,name) in points"
            :key="name"
            :style="{color: point<priceCard ? `red`:`black`}"
        >
          {{ name }}:
          {{ point }}</h1>
        <div class="gamePage__points__buyCards">
          <h1>Покупать за: </h1>
          <input type="radio" id="red" value="Red" v-model="pickedRadio">
          <label for="red">Red</label>
          <br>
          <input type="radio" id="green" value="Green" v-model="pickedRadio">
          <label for="green">Green</label>
          <br>
          <input type="radio" id="blue" value="Blue" v-model="pickedRadio">
          <label for="blue">Blue</label>
        </div>
      </div>
    </div>
  </div>
  <div class="gameOver__Wrap" v-if="isGameOver">
    <div class="gameOver">
      <h1 :style="{color: resGame==='Game Win' ? `darkgreen` : `darkred`}">{{ resGame }}</h1>
      <p>{{ msg }}</p>
      <button @click="isGameOver=false; isStartPage=true">Меню</button>
    </div>
  </div>
>>>>>>> b3739566f99503ac9ba582c52348a158d5c525d0
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
<<<<<<< HEAD
  }

  getRGBA() {
    return `rgba(${this._colors.red},${this._colors.green},${this._colors.blue},${this._colors.alpha})`
  }

  _setBigColor() {
    const value = Object.values(this.getColor());
    const index = value.indexOf(Math.max.apply(null, value));
=======
  }

  _setBigColor() {
    let value = Object.values(this.getColor());
    let index = value.indexOf(Math.max.apply(null, value));
    // return {
    //   Value: value[index],
    //   Color: Object.keys(this.getColor())[index]
    // }
>>>>>>> b3739566f99503ac9ba582c52348a158d5c525d0
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
<<<<<<< HEAD
      pickedRadio: "red",
      priceCard: 100
    }
  },
  components: {
    StartPage,
    GamePage,
    EndPage
=======
      pickedRadio: "Red",
      priceCard: 100
    }
  },
  mounted() {
  },
  updated() {
>>>>>>> b3739566f99503ac9ba582c52348a158d5c525d0
  },
  methods: {
    startGame() {
      this.resetVal();
      this.initCardsHand(this.countHand);
      this.initCardsDeck(this.countDeck);
<<<<<<< HEAD
      this.setPointsStart();
=======
      this.setPointsStart
>>>>>>> b3739566f99503ac9ba582c52348a158d5c525d0
    },
    gameOver() {
      this.resGame = "Game Over";
      if (this.cardsDeck.length === 0) {
        this.msg = "Закончились карты в колоде";
        this.isEndPage = true;
        this.isGamePage = false;
<<<<<<< HEAD
      } else if (Object.values(this.points).every(el => el === 0)) {
=======
      } else if (this.points.Red === 0 && this.points.Green === 0 && this.points.Blue === 0) {
        this.resGame = "Game Over";
>>>>>>> b3739566f99503ac9ba582c52348a158d5c525d0
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
<<<<<<< HEAD
      if (this.cardsDeck.length)
        if (this.points[radio] >= this.priceCard) {
          this.points[radio] -= this.priceCard;
          this.cardsHand.push(this.cardsDeck.shift());
          this.setPoints();
        } else {
          const h1 = document.querySelectorAll(".game-page__points h1")
          console.log(h1);
=======
      if (this.cardsDeck.length !== 0)
        if (this.points[radio] >= this.priceCard) {
          this.points[radio] -= this.priceCard;
          this.cardsHand.push(this.cardsDeck.shift());
          this.setPoints;
        } else {
          const h1 = document.querySelectorAll(".gamePage__points h1")
>>>>>>> b3739566f99503ac9ba582c52348a158d5c525d0
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
<<<<<<< HEAD
    },
    setPointsStart() {
      this.cardsHand.forEach(el => {
        const color = el.getBigColor();
        Object.keys(this.points).forEach(el1 => {
          el1 === color ? this.points[el1] += el.getColor()[color] : false
        })
      });
=======
    }
  },
  computed: {
    setPointsStart() {
      this.cardsHand.forEach(el => {
        const color = el.getBigColor();
        switch (color) {
          case "Red":
            this.points.Red += el.getColor()[color];
            break;
          case "Green":
            this.points.Green += el.getColor()[color];
            break;
          case "Blue":
            this.points.Blue += el.getColor()[color];
            break;
        }
      })
>>>>>>> b3739566f99503ac9ba582c52348a158d5c525d0
    },
    setPoints() {
      const el = this.cardsHand[this.cardsHand.length - 1]
      const color = el.getBigColor();
<<<<<<< HEAD
      Object.keys(this.points).forEach(el1 => {
        el1 === color ? this.points[el1] += el.getColor()[color] : false
      });
=======
      switch (color) {
        case "Red":
          this.points.Red += el.getColor()[color];
          break;
        case "Green":
          this.points.Green += el.getColor()[color];
          break;
        case "Blue":
          this.points.Blue += el.getColor()[color];
          break;
      }
>>>>>>> b3739566f99503ac9ba582c52348a158d5c525d0
    }
  }
}
</script>

<style lang="scss">
@import "src/style/main";

#app {
<<<<<<< HEAD
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
=======
  & .startPage_Wrap {
    height: 100vh;
    width: 100vw;
    display: flex;
    justify-content: center;
    align-items: center;
    background: linear-gradient(45deg, #EECFBA, #C5DDE8);

    & .startPage {
      width: 50%;
      height: 50%;
      display: grid;
      grid-template: 1fr 1fr 1fr/ 1fr 1fr;
      justify-content: center;
      align-items: center;
      border: 2px solid darkred;
      //background-color: #fff;
      background: linear-gradient(-45deg, #EECFBA, #C5DDE8);

      & h1 {
        grid-column: 1/3;
        grid-row: 1/1;
        justify-self: center;
        font-size: 50px;
        font-style: italic;
      }

      &__countHand {
        grid-column: 1/2;
        grid-row: 2/2;
      }

      &__countDeck {
        grid-column: 2/3;
        grid-row: 2/2;
>>>>>>> b3739566f99503ac9ba582c52348a158d5c525d0
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

<<<<<<< HEAD
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
=======
      & button:hover {
        background-color: #00bb00;
        color: #fff;
      }
    }
  }

  & .gamePage_Wrap {
    height: 100vh;
    width: 100vw;
    overflow-x:hidden;
    & .gamePage {
      height: 100vh;
      width: 100vw;
      display: grid;
      grid-template: 0.8fr 1fr/1fr 0.3fr;

      &__cardsDeck {
        grid-column: 1/1;
        grid-row: 1/1;
        justify-self: center;
        align-self: center;

        & img {
          width: 250px;
          height: 270px;
>>>>>>> b3739566f99503ac9ba582c52348a158d5c525d0
        }

        &:checked {
          box-shadow: inset 0 0 0 6px $primary;
        }
      }
<<<<<<< HEAD

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
=======
      &__cardsHand {
        grid-column: 1/3;
        grid-row: 2/2;
        margin: 5px;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;

        &__card {
          height: 230px;
          width: 130px;
          display: flex;
          flex-direction: column;
          border: 1px solid #000;
          filter: invert(1);

          &__wrap {
            width: 150px;
            height: 250px;
            display: flex;
            flex-direction: row;
            flex-wrap: wrap;
            justify-content: center;
            align-items: center;

            filter: invert(1);
            border: 2px solid #fff;
            border-radius: 5%;
            margin: 5px;
          }

          & p {
            margin-left: 5px;
            mix-blend-mode: hard-light;
          }
        }
      }

      &__points {
        grid-column: 2/2;
        grid-row: 1/1;
        align-self: start;

        & .shake {
          -webkit-animation-name: shake;
          -webkit-animation-duration: 0.5s;
          -webkit-transform-origin: 50% 50%;
          -webkit-animation-iteration-count: infinite;
          -webkit-animation-timing-function: linear;
        }

        &__buyCards {
          & input {

            margin: 10px;
          }

          & label {
            font-size: 20px;
            margin: 5px;
          }
>>>>>>> b3739566f99503ac9ba582c52348a158d5c525d0
        }
      }
    }
  }
<<<<<<< HEAD
  .end-page {
    & h1 {
      font-size: 60px;
      margin: 0;
      padding: 0;
=======

  & .gameOver {
    width: 50%;
    height: 50%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    border:2px solid #000;
    //background-color: #fff;
    background: linear-gradient(-45deg, #EECFBA, #C5DDE8);

    & h1 {
      font-size: 60px;
      margin:0 ;
      padding:0;
>>>>>>> b3739566f99503ac9ba582c52348a158d5c525d0
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
<<<<<<< HEAD
=======
    }
    &__Wrap {
      height: 100vh;
      width: 100vw;
      position: fixed;
      top: 0;
      z-index: 99;
      background: linear-gradient(45deg, #EECFBA, #C5DDE8);
      display: flex;
      justify-content: center;
      align-items: center;
>>>>>>> b3739566f99503ac9ba582c52348a158d5c525d0
    }

  }
}

@-webkit-keyframes shake {
  0% {
    -webkit-transform: translate(2px, 1px) rotate(0deg);
  }
  10% {
    -webkit-transform: translate(-1px, -2px) rotate(-1deg);
  }
  20% {
    -webkit-transform: translate(-3px, 0px) rotate(1deg);
  }
  30% {
    -webkit-transform: translate(0px, 2px) rotate(0deg);
  }
  40% {
    -webkit-transform: translate(1px, -1px) rotate(1deg);
  }
  50% {
    -webkit-transform: translate(-1px, 2px) rotate(-1deg);
  }
  60% {
    -webkit-transform: translate(-3px, 1px) rotate(0deg);
  }
  70% {
    -webkit-transform: translate(2px, 1px) rotate(-1deg);
  }
  80% {
    -webkit-transform: translate(-1px, -1px) rotate(1deg);
  }
  90% {
    -webkit-transform: translate(2px, 2px) rotate(0deg);
  }
  100% {
    -webkit-transform: translate(1px, -2px) rotate(-1deg);
  }
}
</style>
