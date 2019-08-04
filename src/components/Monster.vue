<template>
  <div>
    <h2>{{ monsters[0].name }}</h2>

    <div id="monster-health">
      <div v-bind:style="{width: monsters[0].currentHP + '%'}"></div>
    </div>
    <p>{{monsters[0].currentHP}} / {{monsters[0].maxHP}} </p>
  </div>
</template>

<script>
export default {
  name: "Monster",
  props: ["monsters"]
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
