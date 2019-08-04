<template>
  <div id="app">
    <Monster v-bind:currentMonster="currentMonster" :monsters="monsters" />
    <Player v-bind:player="player" :currentPlayer="currentPlayer" />
    <Controls
      v-on:atk-method="basicAtk"
      v-on:doubleAtk-method="doubleAtk"
      v-on:healthPot-method="healthPot"
      v-on:setMonsterLvl-method="setMonsterLvl"
    />
    <BattleLog v-bind:battleLog="battleLog" />
  </div>
</template>

<script>
import Monster from "./components/Monster";
import Player from "./components/Player";
import Controls from "./components/Controls";
import BattleLog from "./components/BattleLog";
import { setTimeout } from "timers";

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
      currentMonster: {},
      currentPlayer: {},
      player: {
        lvl: 1,
        currentHP: 100,
        maxHP: 100,
        hpPot: 5,
        atkPot: 5,
        quickAtk: 3,
        specialAtk: 1
      },
      monsters: [
        {
          lvl: 1,
          name: "Monster Uno",
          currentHP: 100,
          maxHP: 100,
          attack: 15
        },
        {
          lvl: 2,
          name: "Durian Databaser",
          currentHP: 110,
          maxHP: 120,
          attack: 20
        },
        {
          lvl: 3,
          name: "Salty P",
          currentHP: 150,
          maxHP: 150,
          attack: 30
        }
      ]
    };
  },
  methods: {
    setMonsterLvl() {
      this.currentMonster = this.monsters[0];
      console.log("set monster level");
    },
    monsterAtk() {
      if (this.monsters[0].currentHP <= 0) {
        this.monsters[0].currentHP = 0;
        this.battleLog.push(`${this.monsters[0].name} DED`);
        this.setMonsterLvl();
      } else {
        setTimeout(() => {
          this.player.currentHP -= this.monsters[0].attack;
          console.log("player", this.player.currentHP);
          this.battleLog.push(`you've been attacked `);
          if (this.player.currentHP <= 0) {
            this.player.currentHP = 0;
            this.battleLog.push(`U DED!`);
          }
        }, 500);
      }
    },
    basicAtk() {
      this.monsters[0].currentHP -= 10;
      console.log("monster", this.monsters[0].currentHP);
      this.battleLog.push(`you've attacked for ${10} damage`);

      this.monsterAtk();
    },
    doubleAtk() {
      if (this.player.quickAtk > 0) {
        this.monsters[0].currentHP -= 8;

        setTimeout(() => {
          this.monsters[0].currentHP -= 8;
        }, 300);
        console.log("monster", this.monsters[0].currentHP);

        this.monsterAtk();
        this.player.quickAtk -= 1;

        console.log(this.player.quickAtk);

        this.battleLog.push(`you've attacked twice for 7 damamge each`);
      } else {
        this.battleLog.push(
          `you've used the maximum number of double attacks this turn`
        );
      }
    },
    healthPot() {
      if (
        this.player.currentHP <= this.player.maxHP - 20 &&
        this.player.hpPot > 0
      ) {
        this.player.currentHP += 20;
        this.player.hpPot -= 1;
        console.log(this.player.currentHP);
        console.log(this.player.hpPot);
        this.battleLog.push(`you healed for 20 HP`);
      } else {
        this.battleLog.push(`you cannot use this item`);
      }
    },
    attackPot() {}
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
