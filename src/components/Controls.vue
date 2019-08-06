<template>
  <div class="btn-container">
    <button @click="$emit('atk-method')">Attack</button>
    <button @click="$emit('doubleAtk-method')">Double Attack</button>
    <button @click="$emit('healthPot-method')">Health Potion</button>
    <button @click="$emit('specialAtk-method')">Special Attack</button>
    <button @click="$emit('setMonsterLvl-method')">start</button>
    <button @click="$emit('restart-method')">restart</button>
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