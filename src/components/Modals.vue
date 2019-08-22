<template>
  <div>
    <modal name="play-dead" :width="320" :height="500" :clickToClose="false">
      <div class="player-dead-modal">
        <div class="player-dead-modal__header">
          <img class="player-dead-modal__header__skull-one" src="../assets/Skull.svg" alt="skulls" />
          <h2>YOU DED!</h2>
          <img class="player-dead-modal__header__skull-two" src="../assets/Skull.svg" alt="skulls" />
        </div>
        <img class="player-dead-modal__img" src="../assets/Tombstone.svg" alt="tombstone" />
        <button class="restart-btn" @click="$emit('gameResetModal-method')">Restart Game</button>
      </div>
    </modal>

    <modal name="monster-dead" :width="320" :height="500" :clickToClose="false">
      <div class="monster-dead-modal">
        <h2>YOU HAVE DEFEATED SALTY P!!</h2>
        <img class="monster-dead-modal__img" src="../assets/Ice-Cream.svg" alt="tombstone" />
        <button class="restart-btn" @click="$emit('gameEnd-method')">Play Again!</button>
        <p class="monster-dead-modal__plug">
          Check out more of my work at
          <a href="http://www.JoshTag.com">www.JoshTag.com</a>
        </p>
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
    height: 250px;
    margin: 10px 0;

    @media screen and (min-width: 600px) {
      height: 330px;
    }
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
    height: 300px;
    margin: 10px 0;
    animation: wobble-hor-bottom 4s infinite ease-in-out;
  }

  &__plug {
    padding: 10px 16px;
    text-align: center;

    a {
      text-decoration: none;
      color: #303841;
    }
  }
}

.restart-btn:hover {
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

@keyframes wobble-hor-bottom {
  0%,
  100% {
    transform: translateX(0%);
    transform-origin: 50% 50%;
  }
  15% {
    transform: translateX(-30px) rotate(-6deg);
  }
  30% {
    transform: translateX(15px) rotate(6deg);
  }
  45% {
    transform: translateX(-15px) rotate(-3.6deg);
  }
  60% {
    transform: translateX(9px) rotate(2.4deg);
  }
  75% {
    transform: translateX(-6px) rotate(-1.2deg);
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