<template>
  <div>
    <h2>Player</h2>

    <div id="player-health">
      <div v-bind:style="{width: playerHealthBar + '%'}"></div>
    </div>
      <p > {{currentPlayer.currentHP}} / {{currentPlayer.maxHP}} </p>
  </div>
</template>

<script>
export default {
  name: "Player",
  props: [ "currentPlayer", "player" ],
  computed: {
    playerHealthBar() {
      return Math.round(( this.currentPlayer.currentHP / this.currentPlayer.maxHP ) * 100);
    }
  }
};
</script>

<style lang="scss" scoped>
  #player-health {
    width: 200px;
    height: 20px;
    border: 2px solid black;
  }

  #player-health div {
    height: 100%;
    background-color: crimson;
  }

</style>
