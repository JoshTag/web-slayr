<template>
  <div id="app">
    <!-- uncomment to test modal/game lvl-->
    <!-- <button v-on:click="showGameEnd">End game modal</button> -->
    <!-- <button v-on:click="showDeadModal">death modal</button> -->
    <!-- <button style="position: absolute" v-on:click="setMonsterLvl">Monster LVL</button> -->
    <StartPage v-bind:startGame="startGame" v-on:startGame-method="setGameData" v-on:showGameAssets-method="showGameAssets" />
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
        v-on:sqlAtk-method="sqlAtk"
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
import StartPage from "./components/StartPage";
import Modals from "./components/Modals";

export default {
  name: "app",
  components: {
    Monster,
    Player,
    Controls,
    BattleLog,
    Modals,
    BattleStats,
    StartPage
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
              `You've been attacked for ${this.currentMonster.attack}`
            );
            this.checkLoss();
          }
        }, 300);
      }
    },
    monsterAtkThree() {
      let damage = [60, 60, 60, 60, 60, 60, 60, 150, 150, 150];
      let attackRandom = damage[Math.floor(Math.random() * damage.length)];

      this.currentPlayer.currentHP -= attackRandom;
      this.battleLog.push(`You've been attacked for ${attackRandom}`);

      this.checkLoss();
    },
    monsterAtkFour() {
      let attack = (this.currentMonster.increaseAtkBase += 25);

      this.currentPlayer.currentHP -= attack;
      this.battleLog.push(`You've been attacked for ${attack}`);

      this.checkLoss();
    },
    monsterAtkFive() {
      let attackMethods = [1, 2, 3];
      let method =
        attackMethods[Math.floor(Math.random() * attackMethods.length)];

      if (method === 1) {
        this.atkNormal(500);
      } else if (method === 2) {
        this.attackHeal(200, 2500);
      } else {
        this.attackHealth(200, 0.5);
      }

      this.checkLoss();
    },
    monsterAtkSix() {
      let attackMethods = [1, 2, 3, 4];
      let method =
        attackMethods[Math.floor(Math.random() * attackMethods.length)];

      if (method === 1) {
        this.atkNormal(1000);
      } else if (method === 2) {
        this.attackHeal(500, 5000);
      } else {
        this.attackHealth(500, .75);
      }

      this.checkLoss();
    },
    attackHealth(dmgFloor, percent) {
      let attackDamage =
        (this.currentMonster.maxHP - this.currentMonster.currentHP) * percent;

      let roundedDamage = Math.round(attackDamage);

      this.currentPlayer.currentHP -=
        roundedDamage < dmgFloor ? dmgFloor : roundedDamage;
      this.battleLog.push(
        `You've been attacked for ${
          roundedDamage < dmgFloor ? dmgFloor : roundedDamage
        }`
      );
      this.checkLoss();
    },
    attackHeal(dmgHeal, hpReset) {
      this.currentPlayer.currentHP -= dmgHeal;
      this.battleLog.push(`You've been attacked for ${dmgHeal}`);

      if (
        this.currentMonster.currentHP <=
        this.currentMonster.maxHP - dmgHeal
      ) {
        this.currentMonster.currentHP += dmgHeal;
        this.battleLog.push(
          `${this.currentMonster.name} has healed for ${dmgHeal}`
        );
      } else {
        this.currentMonster.currentHP = hpReset;
        this.battleLog.push(
          `${this.currentMonster.name} has healed to full health`
        );
      }
      this.checkLoss();
    },
    atkNormal(dmg) {
      this.currentPlayer.currentHP -= dmg;
      this.checkLoss();
      this.battleLog.push(`You've been attacked for ${dmg} damage`);

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
          `You've attacked for ${this.currentPlayer.attack} damage`
        );
      }

      this.monsterAtk();
    },
    basicAtkThree() {
      let damageRange = [1, 50, 50, 50, 50];
      let attackRandom =
        damageRange[Math.floor(Math.random() * damageRange.length)];

      this.currentMonster.currentHP -= attackRandom;
      this.battleLog.push(`You've attacked for ${attackRandom} damage`);
    },
    basicAtkFour() {
      let DamageMiss = [0, 0, 75, 75, 75];
      let attackRandom =
        DamageMiss[Math.floor(Math.random() * DamageMiss.length)];

      this.currentMonster.currentHP -= attackRandom;

      this.battleLog.push(
        attackRandom === 0
          ? "You missed!"
          : `You've attacked for ${attackRandom} damage`
      );
    },
    basicAtkFive() {
      let DamageMiss = [1, this.currentPlayer.damage, this.currentPlayer.damage];
      let attackRandom =
        DamageMiss[Math.floor(Math.random() * DamageMiss.length)];

      this.currentMonster.currentHP -= attackRandom;

      this.battleLog.push(`You've attacked for ${attackRandom} damage`);
    },
    basicAtkSix() {
      let DamageMiss = [1, this.currentPlayer.damage, this.currentPlayer.damage];
      let attackRandom =
        DamageMiss[Math.floor(Math.random() * DamageMiss.length)];

      this.currentMonster.currentHP -= attackRandom;
      this.battleLog.push(`You've attacked for ${attackRandom} damage`);
    },
    sqlAtk() {
      if (this.currentPlayer.sqlAtkLeft > 0) {
        this.currentMonster.currentHP -= this.currentPlayer.sqlAtk;

        this.monsterAtk();
        this.currentPlayer.sqlAtkLeft -= 1;

        this.battleLog.push(
          `You've attacked with an SQL Injection for ${this.currentPlayer.sqlAtk} damage`
        );
      } else {
        this.battleLog.push(
          `You've used the maximum number of SQL Injections this turn`
        );
      }
    },
    specialAtk() {
      if (this.currentPlayer.spcAtkLeft >= 1) {
        this.currentMonster.currentHP -= this.currentPlayer.specialAtk;
        this.currentPlayer.spcAtkLeft -= 1;
        this.battleLog.push(
          `You've DDoS'd ${this.currentMonster.name} for ${this.currentPlayer.specialAtk} damage`
        );

        this.monsterAtk();
      } else {
        this.battleLog.push("You cannot use this move anymore");
      }
    },
    healthPot() {
      if (
        this.currentPlayer.currentHP <=
          this.currentPlayer.maxHP - this.currentPlayer.healing &&
        this.currentPlayer.hpPot > 0
      ) {
        this.currentPlayer.currentHP += this.currentPlayer.healing;
        this.currentPlayer.hpPot -= 1;
        this.battleLog.push(`You healed for ${this.currentPlayer.healing} HP`);
      } else {
        this.battleLog.push(`You cannot use this item`);
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
    showGameAssets() {
      this.$modal.show("game-assets");
    },
    hideGameAssets() {
      this.$modal.hide("game-assets");
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
        "You're a new web developer looking through the caves of code. \
        Defeat all five coding monsters to win ice cream."
      ];
      this.currentMonster = {
        lvl: 1,
        name: "Cat-a-pus",
        currentHP: 100,
        maxHP: 100,
        attack: 20,
        description:
          "This Git Hub Cat-a-pus will attack you with merge conflicts for 20hp",
        image: require("./assets/Cat.svg")
      };
      this.currentPlayer = {
        lvl: 1,
        name: "Bootcamp Student",
        currentHP: 100,
        maxHP: 100,
        attack: 10,
        sqlAtk: 20,
        sqlAtkLeft: 3,
        specialAtk: 25,
        spcAtkLeft: 1,
        hpPot: 3,
        damage: 10,
        healing: 20,
        image: require("./assets/Player-one.png")
      };
      this.gameData = {
        player: [
          {
            lvl: 2,
            name: "Junior Dev",
            currentHP: 200,
            maxHP: 200,
            attack: 25,
            sqlAtk: 30,
            sqlAtkLeft: 3,
            specialAtk: 50,
            spcAtkLeft: 1,
            hpPot: 4,
            damage: 20,
            healing: 20,
            image: require("./assets/Player-two.png")
          },
          {
            lvl: 3,
            name: "Senior Dev",
            currentHP: 500,
            maxHP: 500,
            sqlAtk: 50,
            sqlAtkLeft: 3,
            specialAtk: 70,
            spcAtkLeft: 1,
            hpPot: 5,
            damage: 40,
            healing: 150,
            image: require("./assets/Player-three.png")
          },
          {
            lvl: 4,
            name: "Full-Stack Dev",
            currentHP: 1000,
            maxHP: 1000,
            sqlAtk: 100,
            sqlAtkLeft: 5,
            specialAtk: 150,
            spcAtkLeft: 1,
            hpPot: 10,
            damage: 60,
            healing: 300,
            image: require("./assets/Player-four.png")
          },
          {
            lvl: 5,
            name: "Product Manager",
            currentHP: 2500,
            maxHP: 2500,
            sqlAtk: 300,
            sqlAtkLeft: 5,
            specialAtk: 500,
            spcAtkLeft: 1,
            hpPot: 20,
            damage: 200,
            healing: 400,
            image: require("./assets/Player-five.png")
          },
          {
            lvl: 6,
            name: "CTO",
            currentHP: 5000,
            maxHP: 5000,
            sqlAtk: 600,
            sqlAtkLeft: 5,
            specialAtk: 1000,
            spcAtkLeft: 1,
            hpPot: 25,
            damage: 300,
            healing: 1000,
            image: require("./assets/Player-six.png")
          }
        ],
        monsters: [
          {
            lvl: 2,
            name: "Fail Whale",
            currentHP: 200,
            maxHP: 200,
            attack: 40,
            description:
              "Twitters long lost 404 page fail whale will attack you for 40hp... \
              The last four got lost somewhere ¯\\_(ツ)_/¯",
            image: require("./assets/Whale.svg")
          },
          {
            lvl: 3,
            name: "Half Dead Haker",
            currentHP: 500,
            maxHP: 500,
            description:
              "This hacker is inconsitent with his skills... cause he's half dead. \
            He has a 70% chance to hack you for 60hp, \
            and 30% chance for 150hp. \
            You also have 20% chance to only damage him for 1hp",
            image: require("./assets/Hacker.svg")
          },
          {
            lvl: 4,
            name: "Database Slug",
            currentHP: 1000,
            maxHP: 1000,
            increaseAtkBase: 75,
            description:
              "This slug constantly collects more data on you. \
              It will attack you for 100hp and each subsequent attack will increase by 25. \
              You also have a 20% chance to miss your attack",
            image: require("./assets/data.svg")
          },
          {
            lvl: 5,
            name: "Spicy P",
            currentHP: 2500,
            maxHP: 2500,
            description:
              "Spicy P uses his spice to spice his developing skills! \
            It can attack you for 500hp, \
            or attack you for 200 damage and heal for 200, \
            or can attack you for 50% of its missing health. \
            You also have a chance to only do 1 dmg",
            image: require("./assets/Pepper.svg")
          },
          {
            lvl: 6,
            name: "Salty P",
            currentHP: 5000,
            maxHP: 5000,
            description: "ITS SALTY P!",
            image: require("./assets/Salt.svg")
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
  background: url("./assets/Background.png");
  background-repeat: no-repeat;
  background-size: 770px 100vh;
  background-attachment: fixed;
  background-position: center;

  @media screen and (min-width: 600px) {
    background-size: 100vw 100vh;
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
    justify-content: space-between;
    width: 300px;
    margin: 0 auto 10px;

    @media screen and (min-width: 600px) {
      width: 600px;
      justify-content: space-between;
      margin: 0 auto;
    }

    @media screen and (min-width: 1023px) {
      width: 750px;
      justify-content: space-between;
      margin: 0 auto 0;
    }
  }
}
</style>
