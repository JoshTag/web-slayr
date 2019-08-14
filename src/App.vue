<template>
  <div id="app">
    <!-- uncomment to test modal -->
    <!-- <button  v-on:click="showGameEnd">test</button>
    <button  v-on:click="showDeadModal">test</button> -->
    <div class="homescreen" v-if="!startGame">
      <img class="homescreen__logo-img" src="./assets/logo.png" alt="logo" />
      <button class="homescreen__start-btn" v-on:click="setGameData">Start Game</button>
    </div>
    <div class="main" v-if="startGame">
      <BattleLog class="main__battlelog" v-bind:battleLog="battleLog" />
      <BattleStats
        class="main__battlestats"
        v-bind:currentMonster="currentMonster"
        :currentPlayer="currentPlayer"
      />
      <div class="main__health-containers">
        <Monster v-bind:currentMonster="currentMonster" />
        <Player v-bind:currentPlayer="currentPlayer" />
      </div>
      <Controls
        v-bind:currentPlayer="currentPlayer"
        v-on:atk-method="basicAtk"
        v-on:doubleAtk-method="doubleAtk"
        v-on:healthPot-method="healthPot"
        v-on:setMonsterLvl-method="setMonsterLvl"
        v-on:specialAtk-method="specialAtk"
        v-on:reset-method="setGameData"
        v-on:show-method="showDeadModal"
      />
    </div>
    <Modals v-on:gameResetModal-method="gameResetModal" v-on:gameEnd-method="gameEndModal" />
  </div>
</template>

<script>
import Monster from "./components/Monster";
import Player from "./components/Player";
import Controls from "./components/Controls";
import BattleLog from "./components/BattleLog";
import BattleStats from "./components/BattleStats";
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
    Modals,
    BattleStats
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
      } else if (method === 2) {
        this.attackHeal(200, 2500);
      } else {
        this.attackHealth(200, 0.4);
      }

      this.checkLoss();
    },
    monsterAtkSix() {
      let attackMethods = [1, 2, 3, 4];
      let method =
        attackMethods[Math.floor(Math.random() * attackMethods.length)];

      if (method === 1) {
        this.attackLoop(100);
      } else if (method === 2) {
        this.attackHeal(400, 5000);
      } else {
        this.attackHealth(500, 0.75);
      }

      this.checkLoss();
    },
    attackHealth(dmgFloor, percent) {
      let attackDamage =
        (this.currentMonster.maxHP - this.currentMonster.currentHP) * percent;

      let roundedDamage = Math.round(attackDamage);

      this.currentPlayer.currentHP -=
        roundedDamage < dmgFloor ? dmgFloor : roundedDamage;
      this.battleLog.push(`You've been attacked for ${roundedDamage < dmgFloor ? dmgFloor : roundedDamage}`)
      this.checkLoss();
    },
    attackHeal(dmgHeal, hpReset) {
      this.currentPlayer.currentHP -= dmgHeal;
      this.battleLog.push(`You've been attacked for ${dmgHeal}`)

      if (
        this.currentMonster.currentHP <=
        this.currentMonster.maxHP - dmgHeal
      ) {
        this.currentMonster.currentHP += dmgHeal;
        this.battleLog.push(`${this.currentMonster.name} has healed for ${dmgHeal}`)
      } else {
        this.currentMonster.currentHP = hpReset;
        this.battleLog.push(`${this.currentMonster.name} has healed to full health`)
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
      this.battleLog.push(`You've been attacked for 500 damage`);

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
      this.battleLog.push(`you've attacked for ${attackRandom} damage`);
    },
    basicAtkFour() {
      let DamageMiss = [0, 0, 75, 75, 75];
      let attackRandom =
        DamageMiss[Math.floor(Math.random() * DamageMiss.length)];

      this.currentMonster.currentHP -= attackRandom;

      let message = ( attackRandom === 0
          ? this.battleLog.push("You missed!")
          : this.battleLog.push(`you've attacked for ${attackRandom} damage`));
    },
    basicAtkFive() {
      let DamageMiss = [1, 250];
      let attackRandom =
        DamageMiss[Math.floor(Math.random() * DamageMiss.length)];

      this.currentMonster.currentHP -= attackRandom = 1
        ? attackRandom
        : attackRandom / 2;

      this.battleLog.push(`you've attacked for ${attackRandom} damage`);
    },
    basicAtkSix() {
      let DamageMiss = [1, 350, 350];
      let attackRandom =
        DamageMiss[Math.floor(Math.random() * DamageMiss.length)];

      this.currentMonster.currentHP -= attackRandom;
      this.battleLog.push(`you've attacked for ${attackRandom} damage`);
    },
    doubleAtk() {
      if (this.currentPlayer.dblAtkLeft > 0) {
        this.currentMonster.currentHP -= this.currentPlayer.dblAtk * 2;

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
        this.hpVariable(this.currentPlayer.healing);
      } else if (this.currentPlayer.lvl === 4) {
        this.hpVariable(this.currentPlayer.healing);
      } else if (this.currentPlayer.lvl === 5) {
        this.hpVariable(this.currentPlayer.healing);
      } else if (this.currentPlayer.lvl === 6) {
        this.hpVariable(this.currentPlayer.healing);
      } else {
        this.hpVariable(this.currentPlayer.healing);
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
    gameEndModal() {
      this.hideGameEnd();
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
      if (this.currentMonster.lvl === 5) {
        let increaseLvl = this.currentLvl++;
        this.currentMonster = this.gameData.monsters[increaseLvl];
        this.currentPlayer = this.gameData.player[increaseLvl];
        this.battleLog.push("SPICY P HAS TRANFORMED INTO SALTY P");
      } else {
        let increaseLvl = this.currentLvl++;
        this.currentMonster = this.gameData.monsters[increaseLvl];
        this.currentPlayer = this.gameData.player[increaseLvl];
      }
    },
    setGameData() {
      this.startGame = true;
      this.currentLvl = 0;
      this.battleLog = [
        "You're just a new web developer looking through the caves of code. \
        It is said that if you defeat all five code monsters, you yourself will become the greatest coder of all time!"
      ];
      this.currentMonster = {
        lvl: 1,
        name: "The Merge Conflictor",
        currentHP: 100,
        maxHP: 100,
        attack: 20,
        description:
          "The Merge Conflictor will attack you with merge conflicts for 20hp"
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
        hpPot: 3,
        damage: 10,
        healing: 20
      };
      this.gameData = {
        player: [
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
            hpPot: 4,
            damage: 25,
            healing: 20
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
            hpPot: 5,
            damage: 50,
            healing: 150
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
            hpPot: 10,
            damage: 75,
            healing: 300
          },
          {
            lvl: 5,
            name: "Product Manager",
            currentHP: 2500,
            maxHP: 2500,
            dblAtk: 100,
            dblAtkLeft: 5,
            specialAtk: 500,
            spcAtkLeft: 1,
            hpPot: 20,
            damage: 75,
            healing: 400
          },
          {
            lvl: 6,
            name: "Super Coder",
            currentHP: 5000,
            maxHP: 5000,
            dblAtk: 250,
            dblAtkLeft: 5,
            specialAtk: 1000,
            spcAtkLeft: 1,
            hpPot: 20,
            damage: 350,
            healing: 1000
          }
        ],
        monsters: [
          {
            lvl: 2,
            name: "The Failed to Compiler",
            currentHP: 200,
            maxHP: 200,
            attack: 40,
            description:
              "This monster will make throw failed to compile erros at you for 40hp"
          },
          {
            lvl: 3,
            name: "The Haker",
            currentHP: 500,
            maxHP: 500,
            description:
              "The hacker is inconsitent with his skills. \
            He has a 80% chance to hack you for 60hp, \
            10% chance for 100hp, \
            and 10% chance for 150hp. \
            You also have 20% chance to only damage him for 1hp"
          },
          {
            lvl: 4,
            name: "The Database Damager",
            currentHP: 1000,
            maxHP: 1000,
            increaseAtkBase: 80,
            description:
              "This monster will inject you with SQL injections. \
              It will attack you for 100hp and each subsequent attack will increase by 20. \
              You also havea 20% chance to miss your attack"
          },
          {
            lvl: 5,
            name: "Spicy P",
            currentHP: 2500,
            maxHP: 2500,
            description:
              "Spicy P is the ultimate web developer! \
            It will attack you multiple times for a total of 500hp. \
            It will use callbacks to attack you for 200 damage and heal for 200. \
            Lastly it's DDoS attack will attack you for 40% of its missing health. \
            You also have a chance to only do 1 dmg"
          },
          {
            lvl: 6,
            name: "Salty P",
            currentHP: 5000,
            maxHP: 5000,
            description: "ITS SALTY AF!"
          }
        ]
      };
    }
  }
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  color: #fff;
  font-family: sans-serif;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
  background: url("./assets/cave-background.png");
  background-size: 770px 90vh;
  background-repeat: no-repeat;
  background-color: black;

  @media screen and (min-width: 600px) {
    background-size: 100vw 80vh;
  }
}

h2 {
  font-size: 18px;
  text-align: center;
}

.main {
  height: 100vh;
  display: flex;
  flex-direction: column;

  &__health-containers {
    display: flex;
    width: 300px;
    margin: 0 auto 10px;

    @media screen and (min-width: 600px) {
      width: 600px;
      justify-content: space-between;
      margin: 0 auto 5vh;
    }

    @media screen and (min-width: 1023px) {
      width: 750px;
      justify-content: space-between;
      margin: 0 auto 3vh;
    }
  }
}

.homescreen {
  width: 320px;
  height: 80vh;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  &__start-btn {
    background: #303841;
    border: none;
    width: 200px;
    height: 70px;
    font-size: 24px;
  }

  &__start-btn:hover {
    cursor: pointer;
  }

  &__logo-img {
    width: 100%;
    margin-bottom: 50px;
  }
}
</style>
