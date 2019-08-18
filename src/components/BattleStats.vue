<template>
  <div class="stats-container">
    <div class="stats-container__stats">
      <div>
        <p>ATK DMG: {{ currentPlayer.damage}}</p>
        <p>DBL ATK DMG: {{ dblAtkCalc }}</p>
      </div>
      <div>
        <p>SPC ATK DMG: {{ currentPlayer.specialAtk}}</p>
        <p>HEALING: {{ currentPlayer.healing}}</p>
      </div>
    </div>
    <p class="description">{{ currentMonster.description }}</p>
  </div>
</template>

<script>
export default {
  name: "BattleStats",
  props: ["currentMonster", "currentPlayer"],
  computed: {
    dblAtkCalc() {
      return this.currentPlayer.dblAtk * 2;
    }
  }
};
</script>

<style lang="scss" scoped>
.stats-container {
  width: 320px;
  margin: 0 auto auto;
  text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000,
    1px 1px 0 #000;

  @media screen and (min-width: 600px) {
    width: 500px;
  }

  @media screen and (min-width: 1023px) {
    width: 600px;
  }

  &__stats {
    display: flex;
    flex-direction: row;
    padding-top: 5px;

    @media screen and (min-width: 1023px) {
      font-size: 18px;
      flex-flow: row wrap;
    }

    p {
      margin-right: 20px;
    }
  }
}
.description {
  font-size: 14px;
  padding-top: 5px;

  @media screen and (min-width: 1023px) {
    font-size: 16px;
  }
}
</style>
