<template>
  <div>
    <modal name="play-dead" :width="300" :height="300">
      <h2>YOU DED!</h2>
      <button @click="$emit('gameResetModal-method')">restart</button>
    </modal>
    <modal name="monster-dead">
      <h2>YOU HAVE DEFEATED SALTY P!!</h2>
      <p>Play Again?</p>
      <button @click="$emit('gameResetModal-method')">restart</button>
    </modal>
  </div>
</template>

<script>
export default {
  name: "Modals"
};
</script>
