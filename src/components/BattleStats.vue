<template>
  <div class="stats-container">
    <div class="stats-container__stats">
      <div class="stats-container__stats__dblWrap">
        <p>ATK DMG: {{ currentPlayer.damage}}</p>
        <p>SQL ATK DMG: {{ currentPlayer.sqlAtk }}</p>
      </div>
      <div class="stats-container__stats__dblWrap">
        <p>DDoS ATK DMG: {{ currentPlayer.specialAtk}}</p>
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
      flex-direction: row;
      width: 700px;
    }

    p {
      margin-right: 20px;
    }

    &__dblWrap {
      @media screen and (min-width: 1023px) {
      display: flex;
      font-size: 18px;
      flex-direction: row;
    }
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
