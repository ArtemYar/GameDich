<template>
  <div class="end-page__wrapper">
    <div class="end-page">
      <slot></slot>
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
}
</style>