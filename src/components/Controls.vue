<template>
  <div class="controls-container">
    <div class="controls-container__player">
      <div class="controls-container__player__group">
        <button class="controls-container__btn" @click="$emit('atk-method')">Attack</button>
        <button class="controls-container__btn" @click="$emit('doubleAtk-method')">Double Attack</button>
      </div>
      <div class="controls-container__player__group">
        <button class="controls-container__btn" @click="$emit('specialAtk-method')">Special Attack</button>
        <button class="controls-container__btn" @click="$emit('healthPot-method')">Health Potion</button>
      </div>
    </div>
    <button class="controls-container__btn-restart" @click="$emit('reset-method')">Restart</button>

    <!-- Uncomment to test attack methods -->
    <!-- <button @click="$emit('setMonsterLvl-method')">start</button> -->
  </div>
</template>

<script>
export default {
  name: "Controls"
};
</script>

<style lang="scss" scoped>
.controls-container {
  display: flex;
  flex-direction: row;
  padding-bottom: 20px;
  width: 320px;
  margin: 0 auto;

  @media screen and (min-width: 600px) {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 400px;
  }

  @media screen and (min-width: 1023px) {
    height: 20vh;
  }

  &__btn {
    height: 30px;
    width: 125px;
    background: #303841;
    border: none;
    margin-bottom: 5px;

    @media screen and (min-width: 600px) {
      width: 100%;
      margin: 5px 0 0;
    }
  }

  &__btn:hover {
    cursor: pointer;
  }

  &__btn-restart {
    height: 65px;
    width: 125px;
    background: #303841;
    border: none;

    @media screen and (min-width: 600px) {
      height: 30px;
      width: 250px;
      margin-top: 10px;
    }
  }

  &__player {
    display: flex;
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