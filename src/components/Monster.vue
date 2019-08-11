<template>
  <div>
    <h2>{{ currentMonster.name }}</h2>

    <div id="monster-health">
      <div v-bind:style="{width: this.monsterHealthBar + '%'}"></div>
    </div>
    <p> {{currentMonster.currentHP}} / {{currentMonster.maxHP}} </p>
  </div>
</template>

<script>
export default {
  name: "Monster",
  props: [ "currentMonster" ],
  computed: {
    monsterHealthBar() {
      return Math.round(( this.currentMonster.currentHP / this.currentMonster.maxHP ) * 100);
    }
  }
}
</script>

<style lang="scss" scoped>
  #monster-health {
    width: 200px;
    height: 20px;
    border: 2px solid black;
  }

  #monster-health div {
    height: 100%;
    background-color: crimson;
  }
</style>
