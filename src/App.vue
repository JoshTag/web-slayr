<template>
  <div id="app">
    <Monster v-bind:gameData="gameData" :currentMonster="currentMonster"/>
    <Player v-bind:gameData="gameData" :currentPlayer="currentPlayer"/>
    <Controls
      v-on:atk-method="basicAtk"
      v-on:doubleAtk-method="doubleAtk"
      v-on:healthPot-method="healthPot"
      v-on:setMonsterLvl-method="setMonsterLvl"
      v-on:specialAtk-method="specialAtk"
      v-on:restart-method="restartGame"
    />
    <BattleLog v-bind:battleLog="battleLog" />
  </div>
</template>

<script>
import Monster from "./components/Monster";
import Player from "./components/Player";
import Controls from "./components/Controls";
import BattleLog from "./components/BattleLog";
import initialData from "./Data/InitialData";
import { setTimeout } from "timers";

export default {
  name: "app",
  components: {
    Monster,
    Player,
    Controls,
    BattleLog
  },
  props: ["initialData"],
  data() {
    return {
      battleLog: ["hello"],
      currentLvl: 1,
      currentMonster: {},
      currentPlayer: {},
      gameData: initialData
    };
  },
  methods: {
    setMonsterLvl() {
      this.currentMonster = this.gameData.data.monsters[0];
      this.currentPlayer = this.gameData.data.player[0];

    },
    monsterAtk() {
      if (this.currentMonster.currentHP <= 0) {
        this.currentMonster.currentHP = 0;
        this.battleLog.push(`${this.currentMonster.name} DED`);
        this.setMonsterLvl();
      } else {
        setTimeout(() => {
          this.currentPlayer.currentHP -= this.currentMonster.attack;
          console.log("player", this.currentPlayer.currentHP);
          this.battleLog.push(`you've been attacked `);
          if (this.currentPlayer.currentHP <= 0) {
            this.currentPlayer.currentHP = 0;
            this.battleLog.push(`U DED!`);
          }
        }, 500);
      }
    },
    basicAtk() {
      this.currentMonster.currentHP -= this.currentPlayer.attack;
      console.log("monster", this.currentMonster.currentHP);
      this.battleLog.push(
        `you've attacked for ${this.currentPlayer.attack} damage`
      );

      this.monsterAtk();
    },
    doubleAtk() {
      if (this.currentPlayer.dblAtkLeft > 0) {
        this.currentMonster.currentHP -= this.currentPlayer.dblAtk;

        setTimeout(() => {
          this.currentMonster.currentHP -= this.currentPlayer.dblAtk;
        }, 300);
        console.log("monster", this.currentMonster.currentHP);

        this.monsterAtk();
        this.currentPlayer.dblAtkLeft -= 1;

        console.log(this.currentPlayer.dblAtkLeft);

        this.battleLog.push(
          `you've attacked twice for ${this.currentPlayer.dblAtk} damage each`
        );
      } else {
        this.battleLog.push(
          `you've used the maximum number of double attacks this turn`
        );
      }
    },
    specialAtk() {
      if (this.currentPlayer.spcAtkLeft <= 1 ) {
      this.currentMonster.currentHP -= this.currentPlayer.specialAtk;
      this.currentPlayer.spcAtkLeft -= 1;
      console.log("monster", this.currentMonster.currentHP);
      this.battleLog.push(
        `you've attacked for ${this.currentPlayer.specialAtk} damage`
      );

      this.monsterAtk();
      } else {
        this.battleLog.push("You Cannot use this move anymore")
      }
    },
    healthPot() {
      if (
        this.currentPlayer.currentHP <= this.currentPlayer.maxHP - 20 &&
        this.currentPlayer.hpPot > 0
      ) {
        this.currentPlayer.currentHP += 20;
        this.currentPlayer.hpPot -= 1;
        console.log(this.currentPlayer.currentHP);
        console.log(this.currentPlayer.hpPot);
        this.battleLog.push(`you healed for 20 HP`);
      } else {
        this.battleLog.push(`you cannot use this item`);
      }
    },
    restartGame() {

        // Object.assign(this.$data, reset());
    

      console.log(initialData)
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}
</style>
