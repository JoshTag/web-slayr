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
  height: 100px;
  width: 320px;
  margin: 0 auto;
  border: 2px solid #FFF;
  overflow: auto;
  background: rgba($color: #000000, $alpha: .5);
  padding: 3px;

  @media screen and (min-width: 600px) {
    width: 500px;
    height: 140px;
  }
  @media screen and (min-width: 1023px) {
    width: 600px;
    height: 20vh;
    padding: 5px;
  }
}

.log-text {
  font-size: 14px;
    @media screen and (min-width: 1023px) {
    font-size: 18px;
  }
}
</style>
