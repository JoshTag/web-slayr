<template>
  <div>
    <modal name="play-dead" :width="300" :height="300">
      <div class="player-dead-modal">
        <div class="player-dead-modal__header">
          <img class="player-dead-modal__header__skull-one" src="../assets/skull.png" alt="skulls" />
          <h2>YOU DED!</h2>
          <img class="player-dead-modal__header__skull-two" src="../assets/skull.png" alt="skulls" />
        </div>
        <img class="player-dead-modal__img" src="../assets/tombstone.png" alt="tombstone" />
        <button class="restart-btn" @click="$emit('gameResetModal-method')">Restart Game</button>
      </div>
    </modal>

    <modal name="monster-dead" :width="300" :height="400">
      <div class="monster-dead-modal">
        <h2>YOU HAVE DEFEATED SALTY P!!</h2>
        <p>Heres a spinning corgi</p>
        <img class="monster-dead-modal__img" src="../assets/dawg.gif" alt="tombstone" />
        <p>Play Again?</p>
        <button class="restart-btn" @click="$emit('gameEnd-method')">Restart</button>
      </div>
    </modal>
  </div>
</template>

<script>
export default {
  name: "Modals"
};
</script>

<style lang="scss" scoped>
.player-dead-modal {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;

  h2 {
    color: #303841;
    margin: 0 15px;
    vertical-align: center;
  }

  button {
    background: #303841;
    border: none;
    height: 30px;
    width: 75%;
    margin-top: 15px;
  }

  &__img {
    height: 150px;
    margin: 10px 0;
    animation: tilting 2.5s infinite linear;
  }

  &__header {
    display: flex;
    flex-direction: row;

    img {
      height: 40px;
    }
    
    &__skull-one {
      animation: skulls 3.5s infinite linear;
    }

    &__skull-two {
      animation: skulls 3.5s infinite linear reverse;
      transform: scaleX(-1);
    }
  }
}

.monster-dead-modal {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 30px;

  h2,
  p {
    color: #303841;
  }

  button {
    background: #303841;
    border: none;
    height: 30px;
    width: 75%;
    margin-top: 5px;
  }

  &__img {
    height: 150px;
    margin: 10px 0;
    animation: rotate 6s infinite ease-in-out;
  }
}

.restart-btn:hover{
  cursor: pointer;
  
}

@keyframes rotate {
  0% {
    transform: scaleX(-1);
  }
  50% {
    transform: scaleX(1);
  }
  100% {
    transform: scaleX(-1);
  }
}

@keyframes tilting {
  0% {
    transform: rotate(25deg);
  }
  25% {
    transform: rotate(0deg);
  }
  50% {
    transform: rotate(-25deg);
  }
  75% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(25deg);
  }
}

@keyframes skulls {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>