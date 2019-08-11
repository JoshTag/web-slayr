<template>
  <div id="app">
    <button class="start-btn" v-on:click="setGameData" v-if="!startGame">Start Game</button>
    <Monster v-bind:gameData="gameData" :currentMonster="currentMonster" v-if="startGame" />
    <Player v-bind:gameData="gameData" :currentPlayer="currentPlayer" v-if="startGame" />
    <Controls
      v-on:atk-method="basicAtk"
      v-on:doubleAtk-method="doubleAtk"
      v-on:healthPot-method="healthPot"
      v-on:setMonsterLvl-method="setMonsterLvl"
      v-on:specialAtk-method="specialAtk"
      v-on:reset-method="setGameData"
      v-on:show-method="showDeadModal"
      v-if="startGame"
    />
    <BattleLog v-bind:battleLog="battleLog" v-if="startGame" />
    <Modals v-on:gameResetModal-method="gameResetModal" />
  </div>
</template>

<script>
import Monster from "./components/Monster";
import Player from "./components/Player";
import Controls from "./components/Controls";
import BattleLog from "./components/BattleLog";
import Modals from "./components/Modals";
import initialData from "./Data/InitialData";
import { setTimeout } from "timers";
import { constants } from "crypto";

export default {
  name: "app",
  components: {
    Monster,
    Player,
    Controls,
    BattleLog,
    Modals
  },
  data() {
    return {
      startGame: false,
      currentLvl: 0,
      battleLog: [],
      currentMonster: {},
      currentPlayer: {},
      gameData: {}
    };
  },
  methods: {
    monsterAtk() {
      if (this.currentMonster.currentHP <= 0 && this.currentMonster.lvl === 6) {
        this.currentMonster.currentHP = 0;
        this.battleLog.push(`${this.currentMonster.name} IS DED!`);
        this.showGameEnd();
      } else if (this.currentMonster.currentHP <= 0) {
        this.currentMonster.currentHP = 0;
        this.battleLog.push(`${this.currentMonster.name} IS DED!`);
        this.setMonsterLvl();
      } else {
        setTimeout(() => {
          if (this.currentMonster.lvl === 3) {
            this.monsterAtkThree();
          } else if (this.currentMonster.lvl === 4) {
            this.monsterAtkFour();
          } else if (this.currentMonster.lvl === 5) {
            this.monsterAtkFive();
          } else if (this.currentMonster.lvl === 6) {
            this.monsterAtkSix();
          } else {
            this.currentPlayer.currentHP -= this.currentMonster.attack;
            console.log("player", this.currentPlayer.currentHP);
            this.battleLog.push(
              `you've been attacked for ${this.currentMonster.attack}`
            );
            this.checkLoss();
          }
        }, 300);
      }
    },
    monsterAtkThree() {
      let damage = [60, 60, 60, 60, 60, 60, 60, 60, 100, 150];
      let attackRandom = damage[Math.floor(Math.random() * damage.length)];

      this.currentPlayer.currentHP -= attackRandom;
      console.log("player", this.currentPlayer.currentHP);
      this.battleLog.push(`you've been attacked for ${attackRandom}`);

      this.checkLoss();
    },
    monsterAtkFour() {
      let attack = (this.currentMonster.increaseAtkBase += 20);

      this.currentPlayer.currentHP -= attack;
      this.battleLog.push(`you've been attacked for ${attack}`);

      this.checkLoss();
    },
    monsterAtkFive() {
      let attackMethods = [1, 2, 3];
      let method =
        attackMethods[Math.floor(Math.random() * attackMethods.length)];

      if (method === 1) {
        this.attackLoop(50);
        console.log("attack one");
      } else if (method === 2) {
        this.attackHeal(200, 2500);
        console.log("attack two");
      } else {
        this.attackHealth(200, 0.4);
        console.log("attack tree");
      }
    },
    monsterAtkSix() {
      let attackMethods = [1, 2, 3, 4];
      let method =
        attackMethods[Math.floor(Math.random() * attackMethods.length)];

      if (method === 1) {
        this.attackLoop(100);
        console.log("attack one");
      } else if (method === 2) {
        this.attackHeal(500, 5000);
        console.log("attack two");
      } else {
        this.attackHealth(500, 0.75);
        console.log("attack tree");
      }

      this.checkLoss();
    },
    attackHealth(dmgFloor, percent) {
      let attackDamage =
        (this.currentMonster.maxHP - this.currentMonster.currentHP) * percent;

      let roundedDamage = Math.round(attackDamage);

      this.currentPlayer.currentHP -=
        roundedDamage < dmgFloor ? dmgFloor : roundedDamage;
      this.checkLoss();
    },
    attackHeal(dmgHeal, hpReset) {
      this.currentPlayer.currentHP -= dmgHeal;

      if (
        this.currentMonster.currentHP <=
        this.currentMonster.maxHP - dmgHeal
      ) {
        this.currentMonster.currentHP += dmgHeal;
      } else {
        this.currentMonster.currentHP = hpReset;
      }
      this.checkLoss();
    },
    attackLoop(dmg) {
      setTimeout(() => {
        this.currentPlayer.currentHP -= dmg;
        this.checkLoss();
        setTimeout(() => {
          this.currentPlayer.currentHP -= dmg;
          this.checkLoss();
          setTimeout(() => {
            this.currentPlayer.currentHP -= dmg;
            this.checkLoss();
            setTimeout(() => {
              this.currentPlayer.currentHP -= dmg;
              this.checkLoss();
              setTimeout(() => {
                this.currentPlayer.currentHP -= dmg;
                this.checkLoss();
                setTimeout(() => {
                  this.currentPlayer.currentHP -= dmg;
                  this.checkLoss();
                  setTimeout(() => {
                    this.currentPlayer.currentHP -= dmg;
                    this.checkLoss();
                    setTimeout(() => {
                      this.currentPlayer.currentHP -= dmg;
                      this.checkLoss();
                      setTimeout(() => {
                        this.currentPlayer.currentHP -= dmg;
                        this.checkLoss();
                        setTimeout(() => {
                          this.currentPlayer.currentHP -= dmg;
                          this.checkLoss();
                        }, 100);
                      }, 100);
                    }, 100);
                  }, 100);
                }, 100);
              }, 100);
            }, 100);
          }, 100);
        }, 100);
      }, 100);

      if (this.currentPlayer.currentHP <= 0) {
        this.currentPlayer.currentHP = 0;
        this.battleLog.push(`U DED!`);
      }
    },
    basicAtk() {
      if (this.currentPlayer.lvl === 3) {
        this.basicAtkThree();
      } else if (this.currentPlayer.lvl === 4) {
        this.basicAtkFour();
      } else if (this.currentPlayer.lvl === 5) {
        this.basicAtkFive();
      } else if (this.currentPlayer.lvl === 6) {
        this.basicAtkSix();
      } else {
        this.currentMonster.currentHP -= this.currentPlayer.attack;
        this.battleLog.push(
          `you've attacked for ${this.currentPlayer.attack} damage`
        );
      }

      this.monsterAtk();
    },
    basicAtkThree() {
      let damageRange = [1, 50, 50, 50, 50];
      let attackRandom =
        damageRange[Math.floor(Math.random() * damageRange.length)];

      this.currentMonster.currentHP -= attackRandom;
      console.log("monster", this.currentMonster.currentHP);
      this.battleLog.push(`you've attacked for ${attackRandom} damage`);
    },
    basicAtkFour() {
      let DamageMiss = [0, 0, 75, 75, 75];
      let attackRandom =
        DamageMiss[Math.floor(Math.random() * DamageMiss.length)];

      this.currentMonster.currentHP -= attackRandom;

      let message =
        attackRandom === 1
          ? this.battleLog.push("You missed!")
          : this.battleLog.push(`you've attacked for ${attackRandom} damage`);
    },
    basicAtkFive() {
      let DamageMiss = [1, 250];
      let attackRandom =
        DamageMiss[Math.floor(Math.random() * DamageMiss.length)];

      this.currentMonster.currentHP -= attackRandom = 1
        ? attackRandom
        : attackRandom / 2;
    },
    basicAtkSix() {
      let DamageMiss = [1, 400, 400];
      let attackRandom =
        DamageMiss[Math.floor(Math.random() * DamageMiss.length)];

      this.currentMonster.currentHP -= attackRandom;
    },
    doubleAtk() {
      if (this.currentPlayer.dblAtkLeft > 0) {
        this.currentMonster.currentHP -= this.currentPlayer.dblAtk;

        setTimeout(() => {
          this.currentMonster.currentHP -= this.currentPlayer.dblAtk;
        }, 300);

        this.monsterAtk();
        this.currentPlayer.dblAtkLeft -= 1;

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
        this.battleLog.push(
          `you've attacked for ${this.currentPlayer.specialAtk} damage`
        );

        this.monsterAtk();
      } else {
        this.battleLog.push("You cannot use this move anymore");
      }
    },
    healthPot() {
      if (this.currentPlayer.lvl === 3) {
        this.hpVariable(150);
      } else if (this.currentPlayer.lvl === 4) {
        this.hpVariable(300);
      } else if (this.currentPlayer.lvl === 5) {
        this.hpVariable(500);
      } else if (this.currentPlayer.lvl === 6) {
        this.hpVariable(500);
      } else {
        this.hpVariable(20);
      }
    },
    hpVariable(healing) {
      if (
        this.currentPlayer.currentHP <= this.currentPlayer.maxHP - healing &&
        this.currentPlayer.hpPot > 0
      ) {
        this.currentPlayer.currentHP += healing;
        this.currentPlayer.hpPot -= 1;
        this.battleLog.push(`you healed for ${healing} HP`);
      } else {
        this.battleLog.push(`you cannot use this item`);
      }
    },
    showDeadModal() {
      this.$modal.show("play-dead");
    },
    hide() {
      this.$modal.hide("play-dead");
    },
    showGameEnd() {
      this.$modal.show("monster-dead");
    },
    hideGameEnd() {
      this.$modal.hide("monster-dead");
    },
    gameResetModal() {
      this.hide();
      this.setGameData();
    },
    checkLoss() {
      if (this.currentPlayer.currentHP <= 0) {
        this.currentPlayer.currentHP = 0;
        this.battleLog.push(`U DED!`);
        setTimeout(() => {
          this.showDeadModal();
        }, 100);
      }
    },
    setMonsterLvl() {
      let increaseLvl = this.currentLvl++;
      this.currentMonster = this.gameData.monsters[increaseLvl];
      this.currentPlayer = this.gameData.player[increaseLvl];
    },
    setGameData() {
      this.startGame = true;
      this.currentLvl = 0;
      this.battleLog = [];
      this.currentMonster = {
        lvl: 1,
        name: "The Merge Conflictor",
        currentHP: 100,
        maxHP: 100,
        attack: 20
      };
      this.currentPlayer = {
        lvl: 1,
        name: "Bootcamp Student",
        currentHP: 100,
        maxHP: 100,
        attack: 10,
        dblAtk: 10,
        dblAtkLeft: 3,
        specialAtk: 25,
        spcAtkLeft: 1,
        hpPot: 3
      };
      this.gameData = {
        player: [
          {
            lvl: 1,
            name: "Bootcamp Student",
            currentHP: 100,
            maxHP: 100,
            attack: 10,
            dblAtk: 10,
            dblAtkLeft: 3,
            specialAtk: 25,
            spcAtkLeft: 1,
            hpPot: 3
          },
          {
            lvl: 2,
            name: "Junior Dev",
            currentHP: 200,
            maxHP: 200,
            attack: 25,
            dblAtk: 15,
            dblAtkLeft: 3,
            specialAtk: 50,
            spcAtkLeft: 1,
            hpPot: 4
          },
          {
            lvl: 3,
            name: "Senior Dev",
            currentHP: 500,
            maxHP: 500,
            dblAtk: 25,
            dblAtkLeft: 3,
            specialAtk: 70,
            spcAtkLeft: 1,
            hpPot: 5
          },
          {
            lvl: 4,
            name: "Product Manager",
            currentHP: 1000,
            maxHP: 1000,
            dblAtk: 40,
            dblAtkLeft: 5,
            specialAtk: 200,
            spcAtkLeft: 1,
            hpPot: 10
          },
          {
            lvl: 5,
            name: "Product Manager",
            currentHP: 2500,
            maxHP: 2500,
            dblAtk: 150,
            dblAtkLeft: 5,
            specialAtk: 500,
            spcAtkLeft: 1,
            hpPot: 20
          },
          {
            lvl: 6,
            name: "Super Coder",
            currentHP: 5000,
            maxHP: 5000,
            dblAtk: 250,
            dblAtkLeft: 5,
            specialAtk: 3000,
            spcAtkLeft: 1,
            hpPot: 20
          }
        ],
        monsters: [
          {
            lvl: 1,
            name: "The Merge Conflictor",
            currentHP: 100,
            maxHP: 100,
            attack: 20
          },
          {
            lvl: 2,
            name: "The Failed to Compiler",
            currentHP: 200,
            maxHP: 200,
            attack: 40
          },
          {
            lvl: 3,
            name: "Monster three",
            currentHP: 500,
            maxHP: 500
          },
          {
            lvl: 4,
            name: "The Great Databaser",
            currentHP: 1000,
            maxHP: 1000,
            increaseAtkBase: 80
          },
          {
            lvl: 5,
            name: "Spicy P",
            currentHP: 2500,
            maxHP: 2500
          },
          {
            lvl: 6,
            name: "Salty P",
            currentHP: 5000,
            maxHP: 5000
          }
        ]
      };
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  color: #202030;
  font-family: sans-serif;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
  background: #ebebd3;
}
</style>
