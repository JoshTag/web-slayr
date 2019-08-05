<template>
  <div>
    <div v-for="logs in battleLog.slice().reverse()" :key="logs.id">
      <p class="log-text">{{ logs }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "BattleLog",
  props: ["battleLog"]
};
</script>

<style lang="scss" scoped>
.log-text {
  font-size: 14px;
}
</style>
