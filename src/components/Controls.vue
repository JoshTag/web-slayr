<template>
  <div class="btn-container">
    <button @click="$emit('atk-method')">Attack</button>
    <button @click="$emit('doubleAtk-method')">Double Attack</button>
    <button @click="$emit('healthPot-method')">Health Potion</button>
    <button>Attack Potion</button>
    <button @click="$emit('setMonsterLvl-method')">start</button>
  </div>
</template>

<script>
export default {
  name: "Controls"
};
</script>

<style lang="scss" scoped>
  .btn-container {
    display: flex;
    flex-direction: column;
  }

</style>