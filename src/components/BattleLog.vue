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
  border: 5px double #FFF;
  overflow: hidden;
  background: rgba($color: #000000, $alpha: .5);
  padding: 5px;

  @media screen and (min-width: 600px) {
    width: 500px;
    height: 160px;
  }
  @media screen and (min-width: 1023px) {
    width: 600px;
    height: 20vh;
  }
}

.log-text {
  font-size: 14px;
}
</style>
