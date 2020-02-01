<template>
  <div class="monster-container">
    <h2 class="monster-container__header">{{ currentMonster.name }}</h2>
    <div id="monster-health">
      <div class="current-hp" v-bind:style="{width: this.monsterHealthBar + '%'}"></div>
    </div>
    <p>{{currentMonster.currentHP}} / {{currentMonster.maxHP}}</p>
    <img class="monster-container__img" alt="monster image" v-bind:src="currentMonster.image"/>
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
  width: 120px;
  display: flex;
  flex-direction: column;
  align-items: center;

  @media screen and (min-width: 600px) {
    width: 250px;
  }

  @media screen and (min-width: 1023px) {
    width: 300px;
    height: 300px;
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
      margin-top: 0;
    }
  }
}

.current-hp {
  transition: 0.3s ease;
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
  height: 15px;
  background-color: #e00a0a;
}
</style>
