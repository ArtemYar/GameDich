<template>
  <div class="startPage_Wrap" v-if="isStartPage">
    <div class="startPage">
      <h1>GameDich</h1>
      <label class="startPage__countHand startPage__count">Кол-во карт на руках<input required type="number" v-model="countHand"></label>
      <label class="startPage__countDeck startPage__count">Кол-во карт в колоде<input required type="number" v-model="countDeck"></label>
      <button @click="startGame">Играть</button>
    </div>
  </div>
  <div class="gamePage_Wrap">
    <div class="gamePage">
      <div class="gamePage__cardsDeck">
        <img src="@/img/Deck.png" alt="Deck">
      </div>
      <div class="gamePage__cardsHand">
        <div class="gamePage__cardsHand__card" v-for="i in countHand"></div>
      </div>
      <div class="gamePage__points">100</div>
    </div>
  </div>
</template>

<script>
class Card {
  _max = 255;
  _min = 0;
  _red = 0;
  _green = 0;
  _blue = 0;

  constructor() {
    this._red = Math.floor(Math.random() * (this._max - this._min + 1)) + this._min;
    this._green = Math.floor(Math.random() * (this._max - this._min + 1)) + this._min;
    this._blue = Math.floor(Math.random() * (this._max - this._min + 1)) + this._min;
  }

  getRed() {
    return this._red
  }

  getGreen() {
    return this._green
  }

  getBlue() {
    return this._blue
  }
}


export default {
  name: 'App',
  components: {},
  data() {
    return {
      isStartPage: false,
      countHand: 10,
      countDeck: 10,
      cardsHand:[],
      cardsDeck:[]
    }
  },
  mounted() {
  },
  methods: {
    startGame(){
      this.isStartPage=false;
      this.initCardsHand(this.countHand);
      this.initCardsDeck(this.countDeck);
    },
    initCardsHand(countHand){
      for (let i=0;i<countHand;i++){
        this.countHand.push(new Card());
      }
    },
    initCardsDeck(countDeck){
      for (let i=0;i<countDeck;i++){
        this.countDeck.push(new Card());
      }
    }
  },
  computed: {}
}
</script>

<style lang="scss">
#app {
  & .startPage_Wrap {
    height: 100vh;
    width: 100vw;
    display: flex;
    justify-content: center;
    align-items: center;


    & .startPage {
      width: 50%;
      height: 50%;
      display: grid;
      grid-template: 1fr 1fr 1fr/ 1fr 1fr;
      justify-content: center;
      align-items: center;
      border: 1px solid darkred;

      & h1{
        grid-column: 1/3;
        grid-row: 1/1;
        justify-self: center;
        font-size: 50px;
        font-style: italic;
      }

      &__countHand{
        grid-column: 1/1;
        grid-row: 2/2;


      }
      &__countDeck{
        grid-column: 2/2;
        grid-row:2/2
      }

      &__count{
        display: flex;
        flex-direction: column;
        justify-self: center;
        font-size: 20px;

        & input{
          height: 20px;
          margin-top: 5px;
        }
      }

      & button{
        grid-column: 1/3;
        grid-row: 3/3;
        width: 150px;
        height: 50px;
        justify-self: center;
      }
    }
  }
  & .gamePage_Wrap{
    height: 100vh;
    width: 100vw;

    & .gamePage{
      height: 100vh;
      width: 100vw;
      display: grid;
      grid-template: 0.8fr 1fr/1fr 0.2fr;
      &__cardsDeck{
        grid-column: 1/3;
        grid-row: 1/1;
        justify-self: center;
        align-self: center;
        & img{
          width: 250px;
          height: 270px;
        }

      }
      &__cardsDeck:hover{
        opacity: 0.8;
      }

      &__cardsHand{
        grid-column: 1/1;
        grid-row: 2/2;
        display: flex;
        justify-content: center;
        align-items: center;

        &__card{
          width: 15%;
          min-width: 70px;
          max-width: 200px;
          height: 70%;
          border: 1px solid #000;
        }

        &__card:hover{

          min-width: 70px;
          max-width: 200px;
          margin-bottom:50px ;
        }
      }

      &__points{
        grid-column: 2/2;
        grid-row: 2/2;
      }
    }
  }
}
</style>
