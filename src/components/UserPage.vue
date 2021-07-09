<template>
  <div class="user-page__wrapper">
    <div class="user-page">
        <slot name="user"></slot>
    </div>
  </div>
</template>

<script>

export default {
  name: "UserPage"
}
</script>

<style scoped lang="scss">
.user-page{
  width: 30%;
  height: 65%;

  border: 1px darkred solid;
  background-color: white;
  &__wrapper{
    height: 100vh;
    width: 100vw;
    display:flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    background: linear-gradient(45deg, #EECFBA, #C5DDE8);
  }
  & form{
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    align-items: center;
    & label{
      margin: 10px 15px;
      font-size: 20px;
      align-self: flex-start;
    }

    & input{
      width: 90%;
      margin-bottom:10px ;
      height: 35px;
    }
    & button{
      width: 40%;
      height: 40px;
      margin: 15px;
    }

  }
  &__phone{
    margin: 10px;
    width: 91%;
    height: 40px;
  }

}
</style>