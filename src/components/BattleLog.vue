<template>
  <div class="log-container">
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
.log-container {
  height: 120px;
  width: 320px;
  margin: 0 auto;
  border: 2px solid rgba(0, 0, 0, .8);
  overflow: hidden;
  background: rgba($color: #000000, $alpha: .3);

  @media screen and (min-width: 600px) {
    width: 500px;
    height: 160px;
  }
  @media screen and (min-width: 1023px) {
    width: 600px;
    height: 25vh;
  }
}

.log-text {
  font-size: 14px;
}
</style>
