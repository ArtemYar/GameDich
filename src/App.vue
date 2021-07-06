<template>
  <div class="startPage_Wrap" v-if="isStartPage">
    <div class="startPage">
      <h1>GameDich</h1>
      <label class="startPage__countHand startPage__count">Кол-во карт на руках
        <input type="number" placeholder="Кол-во карт на руках" v-model="countHand">
      </label>
      <label class="startPage__countDeck startPage__count">Кол-во карт в колоде
        <input type="number" placeholder="Кол-во карт в колоде" v-model="countDeck">
      </label>
      <button @click="startGame">Играть</button>
    </div>
  </div>
  <div class="gamePage_Wrap" v-if="isGamePage">
    <div class="gamePage">
      <div class="gamePage__cardsDeck">
        <img src="@/img/Deck.png" alt="Deck" @click="takeCard(pickedRadio);  gameWin(); gameOver()">
        <h1>Карт в колоде: {{ cardsDeck.length }}</h1>
      </div>

      <div class="gamePage__cardsHand">
        <div
            class="gamePage__cardsHand__card"
            v-for="(card, index ) in cardsHand"
            :key="index"
            :style="{backgroundColor: `rgba(${card.getRed()},${card.getGreen()},${card.getBlue()},${card.getAlpha()})`}"
        >
          <p v-for="(color,name) in card.getColor()" :key="name">
            <span
                :style="{color : name===card.getBigColor().Color ? `red`: `black`}"
            >
              {{ name }}: {{ color }}
            </span>
          </p>
        </div>
      </div>
      <div class="gamePage__points">
        <h1 v-for="(point,name) in points" :key="name">{{ name }}: {{ point }}</h1>
        <div class="gamePage__points__buyCards">
          <h1>Покупать за: </h1>
          <input type="radio" id="red" value="Red" checked v-model="pickedRadio">
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

  constructor() {
    this._colors = {
      Red: Math.floor(Math.random() * (this._max - this._min + 1)) + this._min,
      Green: Math.floor(Math.random() * (this._max - this._min + 1)) + this._min,
      Blue: Math.floor(Math.random() * (this._max - this._min + 1)) + this._min,
      Alpha: Math.random().toFixed(2)
    }
  }

  getColor() {
    return this._colors
  }

  getRed() {
    return this._colors.Red
  }

  getGreen() {
    return this._colors.Green
  }

  getBlue() {
    return this._colors.Blue
  }

  getAlpha() {
    return this._colors.Alpha
  }

  getBigColor() {
    let value = Object.values(this.getColor());
    let index = value.indexOf(Math.max.apply(null, value));
    return {
      Value: value[index],
      Color: Object.keys(this.getColor())[index]
    }
  }
}
const priceCard=100;
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
      pickedRadio: "Red"
    }
  },
  mounted() {

  },
  updated() {
    this.setPoints;
  },
  methods: {
    startGame() {
      this.isStartPage = false;
      this.isGamePage = true;
      this.resetVal();
      this.initCardsHand(this.countHand);
      this.initCardsDeck(this.countDeck);
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
        if(this.points[radio]> priceCard){
          this.points[radio]-=priceCard;
          console.log(this.points[radio]);
          this.cardsHand.push(this.cardsDeck.pop());
        }


    },
    resetVal() {
      this.cardsDeck = [];
      this.cardsHand = [];
    }
  },
  computed: {
    setPoints() {
      this.points.Red = 0;
      this.points.Green = 0;
      this.points.Blue = 0;
      this.cardsHand.forEach(el => {
        switch (el.getBigColor().Color) {
          case "Red":
            this.points.Red += el.getBigColor().Value;
            break;
          case "Green":
            this.points.Green += el.getBigColor().Value;
            break;
          case "Blue":
            this.points.Blue += el.getBigColor().Value;
            break;
        }
      })
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
      border: 1px solid darkred;
      background-color: #fff;


      & h1 {
        grid-column: 1/3;
        grid-row: 1/1;
        justify-self: center;
        font-size: 50px;
        font-style: italic;
      }

      &__countHand {
        grid-column: 1/1;
        grid-row: 2/2;


      }

      &__countDeck {
        grid-column: 2/2;
        grid-row: 2/2
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
        color: white;
      }
    }
  }

  & .gamePage_Wrap {
    height: 100vh;
    width: 100vw;

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
        display: flex;
        justify-content: center;
        align-items: center;
        margin: 5px;

        &__card {
          width: 15%;
          min-width: 70px;
          max-width: 200px;
          height: 70%;
          display: flex;
          flex-direction: column;
          flex-wrap: wrap;
          border: 1px solid #000;
          border-radius: 5%;
          mix-blend-mode: difference;

          & span {
            mix-blend-mode: hard-light;
          }
        }

        &__card:hover {

          min-width: 70px;
          max-width: 200px;
          margin-bottom: 50px;
        }
      }

      &__points {
        grid-column: 2/2;
        grid-row: 1/1;
        align-self: start;

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
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    & h1 {
      font-size: 50px;
    }

    & p {
      font-size: 20px;
    }

    &__Wrap {
      height: 100vh;
      width: 100vw;
      position: fixed;
      top: 0;
      z-index: 99;
      background-color: white;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  }

}
</style>
