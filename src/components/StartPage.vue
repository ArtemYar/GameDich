<template>
  <div class="start-page__wrapper">
    <div class="start-page">
      <h1>GameDich</h1>
      <slot></slot>
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