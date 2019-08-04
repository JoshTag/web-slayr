<template>
  <div id="app">
    <Monster v-bind:monsters="monsters" />
    <Player v-bind:player="player" />
    <Controls
      v-on:atk-method="basicAtk"
      v-on:doubleAtk-method="doubleAtk"
      v-on:healthPot-method="healthPot"
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
      currentLvl: 0,
      player: {
        lvl: 1,
        currentHP: 100,
        maxHP: 100,
        hpPot: 5,
        atkPot: 5,
        quickAttack: 3
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
          currentHP: 120,
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
    monsterAtk() {
      setTimeout(() => {
        this.player.currentHP -= this.monsters[0].attack;
        console.log("player", this.player.currentHP);
        this.battleLog.push(`you've been attacked `);
        if (this.player.currentHP <= 0) {
          alert("you ded");
          location.reload();
        }
      }, 500);
    },
    basicAtk() {
      this.monsters[0].currentHP -= 10;
      console.log("monster", this.monsters[0].currentHP);
      this.monsterAtk();

      this.battleLog.push(`you've attacked for ${10} damage`);
    },
    doubleAtk() {
      if (this.player.quickAttack > 0) {
        this.monsters[0].currentHP -= 7;

        setTimeout(() => {
          this.monsters[0].currentHP -= 7;
        }, 300);
        console.log("monster", this.monsters[0].currentHP);

        this.monsterAtk();
        this.player.quickAttack -= 1;

        console.log(this.player.quickAttack);

        this.battleLog.push(`you've attacked twice for 7 damamge each`);
      } else {
        this.battleLog.push(`you've used the maximum number of double attacks this turn`);
      }
    },
    healthPot() {
      if (this.player.currentHP <= this.player.maxHP - 20 && this.player.hpPot > 0) {
        this.player.currentHP += 20;
        this.player.hpPot -= 1;
        console.log(this.player.currentHP);
        console.log(this.player.hpPot);
        this.battleLog.push(`you healed for 20 HP`);
      } else {
        this.battleLog.push(`you cannot use this yet`);
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
