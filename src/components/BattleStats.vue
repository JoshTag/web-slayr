<template>
  <div class="stats-container">
    <div class="stats-container__stats">
      <p>Attack Damage: {{ currentPlayer.damage}}</p>
      <p>Double Attack Damage: {{ currentPlayer.dblAtk}} x 2</p>
      <p>Special Attack Damage: {{ currentPlayer.specialAtk}}</p>
      <p>Healing: {{ currentPlayer.healing}}</p>
    </div>
    <p class="description">{{ currentMonster.description }}</p>
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
  background: rgba($color: #000000, $alpha: .3);

  @media screen and (min-width: 600px) {
    width: 500px;
  }

  @media screen and (min-width: 1023px) {
    width: 600px;
  }

  &__stats {
    display: flex;
    flex-direction: column;
    padding-top: 5px;


    @media screen and (min-width: 1023px) {
      font-size: 18px;
      flex-flow: row wrap;
    }

    p {
      margin-right: 40px;
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
