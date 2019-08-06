<template>
  <div id="app">
    <Monster v-bind:gameData="gameData" :currentMonster="currentMonster" />
    <Player v-bind:gameData="gameData" :currentPlayer="currentPlayer" />
    <Controls
      v-on:atk-method="basicAtk"
      v-on:doubleAtk-method="doubleAtk"
      v-on:healthPot-method="healthPot"
      v-on:setMonsterLvl-method="setMonsterLvl"
      v-on:specialAtk-method="specialAtk"
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
import randomDamage from "./Data/RandomDamage";
import { setTimeout } from "timers";
import { constants } from "crypto";

export default {
  name: "app",
  components: {
    Monster,
    Player,
    Controls,
    BattleLog
  },
  data() {
    return {
      battleLog: [],
      currentLvl: 1,
      inreasedAtk: 40,
      currentMonster: {},
      currentPlayer: {},
      gameData: initialData.data
    };
  },
  methods: {
    monsterAtk() {
      if (this.currentMonster.currentHP <= 0) {
        this.currentMonster.currentHP = 0;
        this.battleLog.push(`${this.currentMonster.name} DED`);
        this.setMonsterLvl();
      } else {
        setTimeout(() => {
          if (this.currentMonster.lvl === 3) {
            this.monsterAtkThree();
          } else if (this.currentMonster.lvl === 4) {
            this.monsterAtkFour();
          } else {
            this.currentPlayer.currentHP -= this.currentMonster.attack;
            console.log("player", this.currentPlayer.currentHP);
            this.battleLog.push(
              `you've been attacked for ${this.currentMonster.attack}`
            );
            if (this.currentPlayer.currentHP <= 0) {
              this.currentPlayer.currentHP = 0;
              this.battleLog.push(`U DED!`);
            }
          }
        }, 500);
      }
    },
    monsterAtkThree() {
      let damage = randomDamage.randomDamage;
      let attackRandom = damage[Math.floor(Math.random() * damage.length)];

      this.currentPlayer.currentHP -= attackRandom;
      console.log("player", this.currentPlayer.currentHP);
      this.battleLog.push(`you've been attacked for ${attackRandom}`);

      if (this.currentPlayer.currentHP <= 0) {
        this.currentPlayer.currentHP = 0;
        this.battleLog.push(`U DED!`);
      }
    },
    monsterAtkFour() {
      let attack = (this.inreasedAtk += 10);

      this.currentPlayer.currentHP -= attack;
      this.battleLog.push(`you've been attacked for ${attack}`);

      if (this.currentPlayer.currentHP <= 0) {
        this.currentPlayer.currentHP = 0;
        this.battleLog.push(`U DED!`);
      }
    },
    basicAtk() {
      if (this.currentPlayer.lvl === 3) {
        this.basicAtkThree();
      }
      if (this.currentPlayer.lvl === 4) {
        this.basicAtkFour();
      } else {
        this.currentMonster.currentHP -= this.currentPlayer.attack;
        console.log("monster", this.currentMonster.currentHP);
        this.battleLog.push(
          `you've attacked for ${this.currentPlayer.attack} damage`
        );
      }

      this.monsterAtk();
    },
    basicAtkThree() {
      let damageRange = [
        20, 21, 22, 23, 24, 25, 25, 25, 25, 25, 26, 27, 28, 29, 30
      ];
      let attackRandom =
        damageRange[Math.floor(Math.random() * damageRange.length)];

      this.currentMonster.currentHP -= attackRandom;
      console.log("monster", this.currentMonster.currentHP);
      this.battleLog.push(`you've attacked for ${attackRandom} damage`);
    },
    basicAtkFour() {
      let DamageMiss = [0, 35];
      let attackRandom =
        DamageMiss[Math.floor(Math.random() * DamageMiss.length)];

      this.currentMonster.currentHP -= attackRandom;

      let message =
        attackRandom === 0
          ? this.battleLog.push("You missed!")
          : this.battleLog.push(`you've attacked for ${attackRandom} damage`);
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
      if (this.currentPlayer.spcAtkLeft >= 1) {
        this.currentMonster.currentHP -= this.currentPlayer.specialAtk;
        this.currentPlayer.spcAtkLeft -= 1;
        console.log("monster", this.currentMonster.currentHP);
        this.battleLog.push(
          `you've attacked for ${this.currentPlayer.specialAtk} damage`
        );

        this.monsterAtk();
      } else {
        this.battleLog.push("You cannot use this move anymore");
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
    setMonsterLvl() {
      //  this.currentMonster = this.gameData.monsters[2];
      //  this.currentPlayer = this.gameData.player[2];

      this.currentPlayer = {
        lvl: 4,
        currentHP: 400,
        maxHP: 400,
        dblAtk: 18,
        dblAtkLeft: 3,
        specialAtk: 50,
        spcAtkLeft: 1,
        hpPot: 5
      };
      this.currentMonster = {
        lvl: 4,
        name: "Monster four",
        currentHP: 400,
        maxHP: 400
      };
    }
    // restartGame() {
    //   this.gameData =
    // }
    // }
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
