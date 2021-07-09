<template>
  <div class="end-page__wrapper">
    <div class="end-page">
      <slot name="end"></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "EndPage"
}
</script>

<style scoped lang="scss">
.end-page {
  width: 50%;
  height: 50%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 2px solid #000;
  background: linear-gradient(-45deg, #EECFBA, #C5DDE8);

  &__wrapper {
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
</style>