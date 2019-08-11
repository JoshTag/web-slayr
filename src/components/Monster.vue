<template>
  <div class="monster-container">
    <h2>{{ currentMonster.name }}</h2>
    <div id="monster-health">
      <div v-bind:style="{width: this.monsterHealthBar + '%'}"></div>
    </div>
    <p>{{currentMonster.currentHP}} / {{currentMonster.maxHP}}</p>
    <img class="monster-container__img" alt="monster image" src="../assets/monster.png" />
  </div>
</template>

<script>
export default {
  name: "Monster",
  props: ["currentMonster"],
  computed: {
    monsterHealthBar() {
      return Math.round(
        (this.currentMonster.currentHP / this.currentMonster.maxHP) * 100
      );
    }
  }
};
</script>

<style lang="scss" scoped>
.monster-container {
  width: 150px;
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

  &__img {
    height: 125px;
    width: 125px;
    transform: scaleX(-1);
    margin-top: 25px;

    @media screen and (min-width: 1023px) {
      width: 175px;
      height: 175px;
      margin-top: 10px;
    }
  }
}

#monster-health {
  width: 150px;
  height: 20px;
  border: 2px solid black;
  @media screen and (min-width: 1023px) {
    width: 200px;
  }
}

#monster-health div {
  height: 100%;
  background-color: #e00a0a;
}
</style>
