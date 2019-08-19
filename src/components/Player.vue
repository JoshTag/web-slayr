<template>
  <div class="player-container">
    <h2 class="player-container__header">{{ currentPlayer.name }}</h2>
    <div id="player-health">
      <div v-bind:style="{width: playerHealthBar + '%'}"></div>
    </div>
    <p>{{currentPlayer.currentHP}} / {{currentPlayer.maxHP}}</p>
    <img class="player-container__img" alt="player image" src="../assets/player.png" />
  </div>
</template>

<script>
export default {
  name: "Player",
  props: ["currentPlayer"],
  computed: {
    playerHealthBar() {
      return Math.round(
        (this.currentPlayer.currentHP / this.currentPlayer.maxHP) * 100
      );
    }
  }
};
</script>

<style lang="scss" scoped>
.player-container {
  width: 120px;
  display: flex;
  flex-direction: column;
  align-items: center;

  @media screen and (min-width: 600px) {
    width: 250px;
  }

  @media screen and (min-width: 1023px) {
    width: 275px;
    height: 275px;
  }

  &__header {
    height: 50px;
    font-size: 16px;
    vertical-align: text-bottom;
    text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000,
      1px 1px 0 #000;

    @media screen and (min-width: 600px) {
      height: 25px;
    }

    @media screen and (min-width: 1023px) {
      font-size: 24px;
      margin-bottom: 10px;
    }
  }

  &__img {
    height: 125px;
    width: 125px;
    margin-top: 25px;

    @media screen and (min-width: 600px) {
      width: 175px;
      height: 175px;
      margin-top: 0px;
    }

    @media screen and (min-width: 1023px) {
      width: 200px;
      height: 200px;
      margin-top: 0px;
    }
  }
}

#player-health {
  width: 150px;
  height: 20px;
  border: 2px solid black;
  @media screen and (min-width: 1023px) {
    width: 200px;
  }
}

#player-health div {
  height: 15px;
  background-color: #e00a0a;
}
</style>
