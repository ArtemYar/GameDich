<template>
  <div class="game-page__wrapper">
    <div class="game-page">
      <div class="game-page__cards-deck">
        <slot name="img"></slot>
        <h1
            :style="{color: cardsDeck.length<2 ? `red`:`black`}"
            v-text="`Карт в колоде: ${cardsDeck.length}`"
        />
      </div>
      <div class="game-page__cards-hand">
        <div class="game-page__card-wrapper"
             v-for="(card, index ) in cardsHand"
             :key="index"
             :style="{backgroundColor: card.getRGBA()}"
        >
          <div
              class="game-page__card"
              :style="{backgroundColor: card.getRGBA()}"
          >
            <p v-for="(color,name) in card.getColor()" :key="name"
               :style="{color : name===card.getBigColor() ? `red`: `black`}"
               v-text="`${name} : ${color}`"
            />
          </div>
        </div>
      </div>
      <div class="game-page__points">
        <h1
            v-for="(point,name) in points"
            :key="name"
            :style="{color: point<priceCard ? `red`:`black`}"
            v-text="`${name} : ${point}`"
        />
        <div class="game-page__buy-card">
          <h1>Покупать за: </h1>
          <slot name="radioButton"></slot>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "GamePage",
  props:[
    'cardsDeck',
    'cardsHand',
    'points',
    'priceCard'
  ]
}
</script>

<style scoped lang="scss">
 .game-page {
  height: 100vh;
  width: 100vw;
  display: grid;
  grid-template: 0.8fr 1fr/1fr 0.3fr;

  &__wrapper {
    height: 100vh;
    width: 100vw;
    overflow-x: hidden;
  }

  &__cards-deck {
    grid-column: 1/1;
    grid-row: 1/1;
    justify-self: center;
    align-self: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    & h1 {
      margin: 0;

    }
  }

  &__cards-hand {
    grid-column: 1/3;
    grid-row: 2/2;
    margin: 5px;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;

    & p {
      margin-left: 5px;
      mix-blend-mode: hard-light;
    }
  }

  &__card {
    height: 230px;
    width: 130px;
    display: flex;
    flex-direction: column;
    border: 1px solid #000;
    filter: invert(1);

    &-wrapper {
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