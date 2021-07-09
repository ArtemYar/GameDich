<template>
  <div class="start-page__wrapper">
    <div class="start-page">
      <h1>GameDich</h1>
      <slot name="start"></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "StartPage"
}
</script>

<style scoped lang="scss">
.start-page {
  width: 50%;
  height: 50%;
  display: grid;
  grid-template: 1fr 1fr 1fr/ 1fr 1fr;
  justify-content: center;
  align-items: center;
  border: 2px solid darkred;
  background: linear-gradient(-45deg, #EECFBA, #C5DDE8);

  &__wrapper {
    height: 100vh;
    width: 100vw;
    display: flex;
    justify-content: center;
    align-items: center;
    background: linear-gradient(45deg, #EECFBA, #C5DDE8);
  }
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


  & h1 {
    grid-column: 1/3;
    grid-row: 1/1;
    justify-self: center;
    font-size: 50px;
    font-style: italic;
  }

  &__cards-hand {
    grid-column: 1/2;
    grid-row: 2/2;
  }

  &__cards-deck {
    grid-column: 2/3;
    grid-row: 2/2;
  }
}
</style>