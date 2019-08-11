<template>
  <div class="stats-container">
    <div class="stats-container__stats">
      <p>Double Attack: {{ currentPlayer.dblAtkLeft}}</p>
      <p>Special Attack: {{ currentPlayer.spcAtkLeft}}</p>
      <p>HP Potions Left: {{ currentPlayer.hpPot}}</p>
    </div>
    <p>Current Monster: {{ currentMonster.lvl }}</p>
  </div>
</template>

<script>
export default {
  name: "BattleStats",
  props: ["currentMonster", "currentPlayer"]
};
</script>

<style lang="scss" scoped>
.stats-container {
  width: 320px;
  margin: 0 auto auto;

  @media screen and (min-width: 600px) {
    width: 400px;
  }

  @media screen and (min-width: 1023px) {
    width: 500px;
  }

  &__stats {
    @media screen and (min-width: 600px) {
      display: flex;
      justify-content: space-between;
    }
  }
}
</style>
