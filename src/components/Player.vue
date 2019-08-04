<template>
  <div>
    <h2>Player</h2>

    <div id="player-health">
      <div v-bind:style="{width: player.currentHP + '%'}"></div>
    </div>
      <p > {{player.currentHP}} / {{player.maxHP}} </p>
  </div>
</template>

<script>
export default {
  name: "Player",
  props: ["player"],
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
