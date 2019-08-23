<template>
  <div class="controls-container">
    <div class="controls-container__player">
      <div class="controls-container__player__group">
        <button class="controls-container__btn" @click="$emit('atk-method')">Attack</button>
        <button class="controls-container__btn" @click="$emit('sqlAtk-method')">SQL Injection: {{ currentPlayer.sqlAtkLeft}}</button>
      </div>
      <div class="controls-container__player__group">
        <button class="controls-container__btn" @click="$emit('specialAtk-method')">DDoS Attack: {{ currentPlayer.spcAtkLeft}}</button>
        <button class="controls-container__btn" @click="$emit('healthPot-method')">Health Potion: {{ currentPlayer.hpPot}}</button>
      </div>
    </div>
    <!-- <button class="controls-container__btn-restart" @click="$emit('reset-method')">Restart</button> -->
  </div>
</template>

<script>
export default {
  name: "Controls",
  props: [ "currentPlayer" ] 
};
</script>

<style lang="scss" scoped>
.controls-container {
  display: flex;
  flex-direction: row;
  width: 320px;
  margin: 0 auto 10px;

  @media screen and (min-width: 600px) {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 400px;
  }

  @media screen and (min-width: 1023px) {
    height: 15vh;
  }

  &__btn {
    height: 30px;
    width: 155px;
    background: #170312;
    border: none;
    margin-bottom: 5px;

    @media screen and (min-width: 600px) {
      width: 100%;
      margin: 15px 0 0;
      font-size: 14px;
    }
  }

  &__btn:hover {
    cursor: pointer;
  }

  &__btn-restart {
    height: 65px;
    width: 125px;
    background: #191308;
    border: none;

    @media screen and (min-width: 600px) {
      height: 30px;
      width: 250px;
      margin-top: 10px;
    }
  }

  &__player {
      display: flex;
      flex-direction: row;
      justify-content: space-between;

    @media screen and (min-width: 600px) {
      width: 100%;
      justify-content: space-between;
    }

    &__group {
      @media screen and (min-width: 600px) {
        flex-direction: row;
        width: 45%;
      }
    }
  }
}
</style>