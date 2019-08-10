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
      increaseLvlFour: 80,
      increaseLvlsix: 300,
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

            // put in check if player ded function
            if (this.currentPlayer.currentHP <= 0) {
              this.currentPlayer.currentHP = 0;
              this.battleLog.push(`U DED!`);
            }
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

      if (this.currentPlayer.currentHP <= 0) {
        this.currentPlayer.currentHP = 0;
        this.battleLog.push(`U DED!`);
      }
    },
    monsterAtkFour() {
      let attack = (this.increaseLvlFour += 20);

      this.currentPlayer.currentHP -= attack;
      this.battleLog.push(`you've been attacked for ${attack}`);

      if (this.currentPlayer.currentHP <= 0) {
        this.currentPlayer.currentHP = 0;
        this.battleLog.push(`U DED!`);
      }
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
        this.attackHealth(200, .4);
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
    },
    attackHealth(dmgFloor, percent) {
      let attackDamage =
        (this.currentMonster.maxHP - this.currentMonster.currentHP) * percent;

      let roundedDamage = Math.round(attackDamage);

      this.currentPlayer.currentHP -=
        roundedDamage < dmgFloor ? dmgFloor : roundedDamage;
      console.log(
        "ATTACK!",
        roundedDamage < dmgFloor ? dmgFloor : roundedDamage
      );
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
    },
    attackLoop(dmg) {
      setTimeout(() => {
        this.currentPlayer.currentHP -= dmg;
        setTimeout(() => {
          this.currentPlayer.currentHP -= dmg;
          setTimeout(() => {
            this.currentPlayer.currentHP -= dmg;
            setTimeout(() => {
              this.currentPlayer.currentHP -= dmg;
              setTimeout(() => {
                this.currentPlayer.currentHP -= dmg;
                setTimeout(() => {
                  this.currentPlayer.currentHP -= dmg;
                  setTimeout(() => {
                    this.currentPlayer.currentHP -= dmg;
                    setTimeout(() => {
                      this.currentPlayer.currentHP -= dmg;
                      setTimeout(() => {
                        this.currentPlayer.currentHP -= dmg;
                        setTimeout(() => {
                          this.currentPlayer.currentHP -= dmg;
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
    // attackPromise(){
    //   return new Promise(function(res, rej) {
    //       this.attackLoop();
    //   })
    // },
    // attackLoop() {
    //   setTimeout(() => {
    //     this.currentPlayer.currentHP -= 50;
    //   }, 100);
    //   setTimeout(() => {
    //     this.currentPlayer.currentHP -= 50;
    //   }, 200);
    //   setTimeout(() => {
    //     this.currentPlayer.currentHP -= 50;
    //   }, 300);
    //   setTimeout(() => {
    //     this.currentPlayer.currentHP -= 50;
    //   }, 400);
    //   setTimeout(() => {
    //     this.currentPlayer.currentHP -= 50;
    //   }, 500);
    //   setTimeout(() => {
    //     this.currentPlayer.currentHP -= 50;
    //   }, 600);
    //   setTimeout(() => {
    //     this.currentPlayer.currentHP -= 50;
    //   }, 700);
    //   setTimeout(() => {
    //     this.currentPlayer.currentHP -= 50;
    //   }, 800);
    //   setTimeout(() => {
    //     this.currentPlayer.currentHP -= 50;
    //   }, 900);
    //   setTimeout(() => {
    //     this.currentPlayer.currentHP -= 50;
    //   }, 1000);

    //   if (this.currentPlayer.currentHP <= 0) {
    //     this.currentPlayer.currentHP = 0;
    //     this.battleLog.push(`U DED!`); }
    // },
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
        console.log("monster", this.currentMonster.currentHP);
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
      if (this.currentPlayer.lvl === 3) {
        this.hpVariable(150);
      } else if (this.currentPlayer.lvl === 4) {
        this.hpVariable(300);
      } else if (this.currentPlayer.lvl === 5) {
        this.hpVariable(500);
      } else if (this.currentPlayer.lvl === 6) {
        this.hpVariable(800);
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
        console.log(this.currentPlayer.currentHP);
        console.log(this.currentPlayer.hpPot);
        this.battleLog.push(`you healed for ${healing} HP`);
      } else {
        this.battleLog.push(`you cannot use this item`);
      }
    },
    setMonsterLvl() {
      this.currentMonster = this.gameData.monsters[5];
      this.currentPlayer = this.gameData.player[5];

      // this.currentPlayer = {
      //   lvl: 6,
      //   currentHP: 5000,
      //   maxHP: 5000,
      //   dblAtk: 250,
      //   dblAtkLeft: 5,
      //   specialAtk: 800,
      //   spcAtkLeft: 1,
      //   hpPot: 20
      // };
      // this.currentMonster = {
      //   lvl: 6,
      //   name: "Monster six",
      //   currentHP: 5000,
      //   maxHP: 5000
      // };
    }
    // setGameData() {
    //   this.gameData = initialData.data
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
