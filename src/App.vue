<template>
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
</template>

<script>
class Card {
  _max = 255;
  _min = 0;
  _colors;
  _bigColor = 0;

  constructor() {
    this._colors = {
      Red: Math.floor(Math.random() * (this._max - this._min + 1)) + this._min,
      Green: Math.floor(Math.random() * (this._max - this._min + 1)) + this._min,
      Blue: Math.floor(Math.random() * (this._max - this._min + 1)) + this._min,
      Alpha: Math.random().toFixed(2)
    }
    this._bigColor = this._setBigColor();
  }

  getColor() {
    return this._colors
  }

  getBigColor() {
    return this._bigColor
  }

  _setBigColor() {
    let value = Object.values(this.getColor());
    let index = value.indexOf(Math.max.apply(null, value));
    // return {
    //   Value: value[index],
    //   Color: Object.keys(this.getColor())[index]
    // }
    return Object.keys(this.getColor())[index];
  }
}

export default {
  name: 'App',
  data() {
    return {
      isStartPage: true,
      isGameOver: false,
      isGamePage: false,
      countHand: 3,
      countDeck: 4,
      cardsHand: [],
      cardsDeck: [],
      points: {
        Red: 0,
        Green: 0,
        Blue: 0,
      },
      resGame: '',
      msg: '',
      pickedRadio: "Red",
      priceCard: 100
    }
  },
  mounted() {
  },
  updated() {
  },
  methods: {
    startGame() {
      this.isStartPage = false;
      this.isGamePage = true;
      this.resetVal();
      this.initCardsHand(this.countHand);
      this.initCardsDeck(this.countDeck);
      this.setPointsStart
    },
    gameOver() {
      if (this.cardsDeck.length === 0) {
        this.resGame = "Game Over";
        this.msg = "Закончились карты в колоде";
        this.isGameOver = true;
        this.isGamePage = false;
      } else if (this.points.Red === 0 && this.points.Green === 0 && this.points.Blue === 0) {
        this.resGame = "Game Over";
        this.msg = "Очки равны нулю ";
        this.isGameOver = true;
        this.isGamePage = false;
      }
    },
    gameWin() {
      if (this.points.Red === this.points.Green && this.points.Red === this.points.Blue) {
        this.resGame = "Game Win";
        this.msg = "Вы выиграли!!"
        this.isGameOver = true;
        this.isGamePage = false;
      }
    },
    initCardsHand(countHand) {
      for (let i = 0; i < countHand; i++) {
        this.cardsHand.push(new Card());
      }
    },
    initCardsDeck(countDeck) {
      for (let i = 0; i < countDeck; i++) {
        this.cardsDeck.push(new Card());
      }
    },
    takeCard(radio) {
      if (this.cardsDeck.length !== 0)
        if (this.points[radio] >= this.priceCard) {
          this.points[radio] -= this.priceCard;
          this.cardsHand.push(this.cardsDeck.shift());
          this.setPoints;
        } else {
          const h1 = document.querySelectorAll(".gamePage__points h1")
          const index = Object.keys(this.points).indexOf(radio);
          h1[index].classList.add("shake")
          setTimeout(() => h1[index].classList.remove('shake'), 500);
        }
    },
    resetVal() {
      this.cardsDeck = [];
      this.cardsHand = [];
      Object.keys(this.points).forEach(el => this.points[el] = 0);
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
    },
    setPoints() {
      const el = this.cardsHand[this.cardsHand.length - 1]
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
    }
  }
}
</script>

<style lang="scss">
@import "src/style/main";

#app {
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
      }

      &__count {
        display: flex;
        flex-direction: column;
        justify-self: center;
        font-size: 20px;

        & input {
          height: 40px;
          margin-top: 5px;
          border: 1px grey solid;
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
        }

        & img:hover {
          opacity: 0.7;
        }
      }
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
        }
      }
    }
  }

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
